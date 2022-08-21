## Implementation Details

# Major Tools Used

- Trivy
- jq
- sed

Trivy was used to do the image scanning. This was also used with the --template flag to make it easy to get a simpler output which can be used to check if an image is SAFE or UNSAFE.

jq was used to created the JSON array containing the image name and its status

sed was used to do some formating and stripping of the initial input, which is a list of images to be scanned.

## How Does it work?

