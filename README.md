Lab 05 — S3 Static Website Portfolio

1. Project Overview

This project deploys a simple, multi-page portfolio website using Amazon S3 static website hosting. 
The site contains:

- `index.html` hosts Home page
- `about.html` hosts information; Education, skills, and interests
- `projects.html` hosts project descriptions, technologies, contributions, and available GitHub links
- `style.css` — shared styling for all pages
- `images/` — Local website illustrations and other image assets



2. Architecture

Visitor's browser -> sends request to S3 website endpoint -> Amazon S3 static website hosting
    
       +--> index.html
       +--> about.html
       +--> projects.html
       +--> style.css
       +--> images/


- **Storage:** An S3 bucket stores the HTML pages, stylesheet, and image assets as objects.
- **Website hosting:** Static website hosting is enabled on the bucket, with `index.html` configured as the index document.
- **Navigation:** Each page links to Home, About, and Projects.
- **Styling and assets:** Each HTML page references the shared `style.css` file. Image paths are relative to the website files.
- **Public access:** If the site is intentionally public, visitors need read access to the website objects. The bucket policy should grant only the permissions required to serve the site.

3. Deployment

a. Created a S3 bucket 
b. Uploaded html, css files and images as a folder.
c. Under the bucket's **Properties**, enable **Static website hosting** and set `index.html` as the index document.
d. Copy the S3 website endpoint shown in the hosting settings.
e. For a public S3 website endpoint, public reads must be allowed ; change Block Public Access settings and adding a bucket policy.
f. Test website, and check all three pages.

4. Security Notes

- Grant only the permissions needed to serve the website. Do not grant public `s3:PutObject` or `s3:DeleteObject`.
- Public-read objects can be viewed by anyone on the internet. Do not upload passwords, access keys, secret keys, private keys, payment details, private documents, or personal records.
- Keep AWS credentials out of HTML, CSS, screenshots, reports, and GitHub.
- Review the bucket contents and policy before submission.
- Hide account identifiers and unrelated sensitive details in evidence screenshots.
- The S3 static website endpoint uses **HTTP**, not HTTPS. For production HTTPS and custom-domain hosting, Amazon CloudFront with a domain is a common approach.
- Bucket policy and Block Public Access settings must be considered together. Block Public Access can prevent public access even when a bucket policy appears to allow it.

5. Cost Notes

S3 is often a suitable low-operations option for a small static portfolio, but it is not automatically free.

Potential cost factors include:

- Stored objects and retained object versions
- Requests to read or list objects
- Data transferred out to visitors
- Enabled features and any additional services, such as CloudFront
- Applicable regional pricing and account-specific Free Tier terms

Cost-control actions:

1. Check AWS pricing for the selected Region and review Billing and Cost Management tools if permitted.
2. Keep only required files and remove unnecessary objects or old versions after assessment, following instructor directions.
3. Avoid enabling extra services unless required.
4. Do not assume all usage is covered by the Free Tier; monitor usage and billing.

 6. Troubleshooting

CSS does not load

- Confirm `style.css` is uploaded at the same level as `index.html`.
- Confirm every page uses the correct link

- S3 object keys are case-sensitive: `style.css` and `Style.css` are different.
- Open the S3 website endpoint followed by `/style.css` to inspect the result.
- If the CSS text appears, check the HTML path and refresh the browser. If the response is `403 Access Denied`, inspect public-access settings and the bucket policy. If it is `404 Not Found`, check the object name and location.


 Website returns Access Denied

- Verify static website hosting is enabled and `index.html` is the configured index document.
- Review Block Public Access and the bucket policy.

7. EC2 vs S3
Amazon EC2 with Apache requires managing the operating system, web server, software updates, server configuration, and instance capacity, whereas Amazon S3 static website hosting requires managing only the bucket, website files, hosting settings, and access permissions. 

EC2 can host static websites and server-side applications with suitable configuration, while S3 static website hosting supports static HTML, CSS, JavaScript, and images but cannot execute server-side code. 

EC2 security involves security groups and server configuration, whereas S3 security relies on bucket policies, IAM, Block Public Access, and object permissions. 

EC2 costs may include instance runtime, EBS storage, data transfer, and public IPv4 charges, while S3 costs depend on storage, requests, data transfer, object versions, and enabled features. 

The S3 website endpoint supports HTTP only, while HTTPS can be configured for EC2 or provided for an S3-hosted website through services such as CloudFront.



