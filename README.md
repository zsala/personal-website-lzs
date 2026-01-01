# Personal Website

A modern, static website showcasing professional experience and skills.

## Features

- Clean, modern design with complementary color scheme (Deep Blue & Warm Coral)
- Fully responsive layout
- Optimized for S3 static hosting
- No external dependencies
- Fast loading times

## Deployment to S3

### Prerequisites
- AWS CLI configured
- S3 bucket created with public read access
- Optional: CloudFront distribution for CDN

### Steps

1. **Create S3 bucket** (if not already created):
   ```bash
   aws s3 mb s3://your-bucket-name
   ```

2. **Configure bucket for static website hosting**:
   ```bash
   aws s3 website s3://your-bucket-name \
     --index-document index.html \
     --error-document index.html
   ```

3. **Set bucket policy for public read access**:
   Create a file `bucket-policy.json`:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::your-bucket-name/*"
       }
     ]
   }
   ```
   
   Apply the policy:
   ```bash
   aws s3api put-bucket-policy --bucket your-bucket-name --policy file://bucket-policy.json
   ```

4. **Upload files to S3**:
   ```bash
   aws s3 sync . s3://your-bucket-name --exclude "*.md" --exclude ".git/*"
   ```

5. **Access your website**:
   Your website will be available at:
   ```
   http://your-bucket-name.s3-website-<region>.amazonaws.com
   ```

### Optional: Custom Domain with CloudFront

1. Create a CloudFront distribution pointing to your S3 bucket
2. Configure your custom domain to point to the CloudFront distribution
3. Enable HTTPS with AWS Certificate Manager

## File Structure

```
.
├── index.html      # Main HTML file
├── styles.css      # CSS stylesheet
└── README.md       # This file
```

## Customization

- Colors can be adjusted in `styles.css` under the `:root` variables
- Content can be modified in `index.html`
- All styling is self-contained in `styles.css`

