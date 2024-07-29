# scripts
useful scripts. bash/python/etc.

---

To remove Docker images that are not being used by any running containers, you can extend the script to include the `docker rmi` command for each unused image. Here’s how you can do it without using the `--force` option:

```bash
docker images --format '{{.Repository}}:{{.Tag}} {{.ID}}' | while read image; do
  image_id=$(echo $image | awk '{print $2}')
  if [ -z "$(docker ps -q --filter ancestor=$image_id)" ]; then
    echo "Removing image: $image"
    docker rmi $image_id
  fi
done
```

This script will:
1. List all images with their repository, tag, and image ID.
2. Check each image to see if it's in use by any running container.
3. If an image is not in use, it prints a message indicating that the image is being removed and then removes the image using `docker rmi` without the `--force` option.
---
