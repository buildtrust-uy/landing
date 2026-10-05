# Deploy to Amazon S3 (AWS Academy)

The `deploy` job in `.github/workflows/deploy.yml` publishes the website on every
push to `main`. It can also be started from **Actions → Deploy to Amazon S3 → Run
workflow**. The runner is `ubuntu-latest`, and the AWS region is `us-east-1`.

## GitHub configuration

Under **Settings → Secrets and variables → Actions**, configure these repository
secrets from the current AWS Academy lab session:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_SESSION_TOKEN`

Configure the repository variable `S3_BUCKET` with the destination bucket name.
Never commit credential values to the repository.

AWS Academy credentials are temporary. When they expire, start or resume the lab,
replace all three GitHub secrets with the current session values, and rerun the
workflow. Existing S3 files do not require valid GitHub secrets to be served, but
availability remains subject to the lab's resource lifecycle.

## Bucket configuration

The destination is a dedicated website bucket in `us-east-1`, with static website
hosting enabled and `index.html` as the index document. Its bucket policy grants
anonymous `s3:GetObject` access to website files only; uploads require AWS
credentials. Bucket ACLs remain disabled and blocked. Account-level public-access
restrictions must also permit this bucket policy.

Only root HTML files and non-hidden files under `assets/` are deployed. Git metadata,
workflow files, documentation, `CNAME`, and macOS metadata are not published.
Synchronization removes obsolete objects from the destination, so this bucket must
remain dedicated to this site.

The S3 website endpoint is HTTP:

`http://<S3_BUCKET>.s3-website-us-east-1.amazonaws.com`

The workflow checks the home page, demo page, CSS, icons, and video after uploading.
The existing GitHub Pages domain and DNS are managed separately.
