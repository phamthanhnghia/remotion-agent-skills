# Lambda Render Reference

AWS Lambda rendering distributes frame rendering across hundreds of parallel Lambda invocations, reducing render time for long or complex compositions from hours to minutes.

## Prerequisites

1. AWS account with appropriate IAM permissions
2. `@remotion/lambda` package installed
3. AWS credentials as environment variables: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION`

```bash
npm install @remotion/lambda
```

## One-time setup

```bash
# Create the necessary AWS roles and policies (interactive)
npx remotion lambda policies validate

# Deploy the Lambda function (run once per region per Remotion version)
npx remotion lambda functions deploy \
  --memory=3009 \
  --timeout=120 \
  --region=us-east-1
```

> 3009 MB memory = 3 vCPUs. This is the recommended configuration for most projects.

## Site deployment

```bash
# Bundle the project and upload to S3
npx remotion lambda sites create \
  --site-name=my-video-project \
  --region=us-east-1

# Update an existing site after code changes
npx remotion lambda sites create \
  --site-name=my-video-project \
  --region=us-east-1
```

The command outputs a `serveUrl` that can be used for rendering without re-uploading.

## Render

```bash
# Basic render
npx remotion lambda render \
  my-video-project \
  MyComposition \
  --region=us-east-1

# With props
npx remotion lambda render \
  my-video-project \
  MyComposition \
  --region=us-east-1 \
  --props='{"title":"Hello","duration":60}'

# Monitor progress
npx remotion lambda renders progress <render-id> --region=us-east-1

# Download output
npx remotion lambda renders s3bucket --region=us-east-1
```

## Node.js API (for programmatic batch rendering)

```ts
import {renderMediaOnLambda, getRenderProgress} from '@remotion/lambda';

const {renderId, bucketName} = await renderMediaOnLambda({
  region: 'us-east-1',
  functionName: 'remotion-render-4-0-0-mem3009mb-disk10240mb-120sec',
  serveUrl: 'https://s3.us-east-1.amazonaws.com/my-bucket/sites/my-video-project/index.html',
  composition: 'MyComposition',
  codec: 'h264',
  inputProps: {title: 'Hello'},
});

// Poll for progress
const progress = await getRenderProgress({renderId, bucketName, functionName, region: 'us-east-1'});
```

## Cost and performance

| Configuration | Render speed | Cost estimate |
| --- | --- | --- |
| 3009 MB (3 vCPUs) | ~10x faster than local | ~$0.05 per minute of video |
| 10240 MB (10 vCPUs) | ~30x faster than local | ~$0.15 per minute of video |

Use Lambda when: the project needs to render in under 5 minutes, or when local rendering takes more than 15 minutes.

Do not use Lambda when: the composition uses local-only resources (disk files not in S3), or the project is still in development.
