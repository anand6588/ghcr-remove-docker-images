# Delete Docker Images from GHCR.IO

This GitHub Actions workflow is designed to delete Docker images from GHCR.IO based on specified tags, wildcard patterns, or untagged images. It can be called from other workflows to automate the cleanup of Docker images in your repository.

## Inputs
- `image` (required): The Docker image name (e.g., owner/repo/image).
- `tags` (optional): A comma-separated list of image tags to delete (e.g., tag1,tag2,tag3).
- `wildcard-tag` (optional): A wildcard pattern to match tags that start with the specified string (e.g., pr-*).
- `remove-all-untagged-images` (optional): A boolean flag to remove all untagged images of the package/image (default: false).

## Example Workflow
```yaml
name: Cleanup Docker Images

on:
  workflow_dispatch:

jobs:
  call-delete-docker-images:
    uses: anand6588/ghcr-remove-docker-images/.github/workflows/action.yml@main
    with:
      image: 'myorg/myrepo/myimage'
      tags: 'latest,dev,staging'
      wildcard-tag: 'pr-*'
      remove-all-untagged-images: true
    secrets:
      GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Details
### Steps in the Workflow
1. **Install jq:** Installs the jq utility for processing JSON data.
2. **Get Package Versions:** Fetches all versions of the specified Docker image from GHCR.IO and stores them in a versions.json file.
3. **Delete Docker Images:** Deletes all images specified in `tags`.  Deletes all untagged images if `remove-all-untagged-images` is set to true. Deletes images matching the wildcard pattern specified in `wildcard-tag`.

### Environment Variables
- `GITHUB_TOKEN`: The GitHub token required to authenticate API requests to GHCR.IO. Ensure this token has the necessary permissions to delete packages.

### Functions Used
- `fetch_versions`: Fetches all versions of the Docker image from GHCR.IO.
- `delete_version`: Deletes a specific version of the Docker image by its ID.

## Troubleshooting
- Ensure that the `GH_TOKEN` secret has the necessary permissions to delete Docker images.
- Check the workflow logs for any error messages returned by the GitHub API.
- Verify the correctness of the image name, tags, and wildcard patterns provided as inputs.

## License
This project is licensed under the MIT License.
