# Jekyll in a Docker Container

Why not to use `docker pull jekyll/jekyll` or `docker pull bretfisher/jekyll-serve`?

- Instead of mounting local folder as `bretfisher/jekyll-serve` I wanted to pull files from git repository of my choice.

So, this does that. 

I am using docker container to deploy develop branch of my blog to preview changes in  a cloud before merging it to master. once changes tested in azure container instance and merged, github pages picking them up and publishing to gtrifonov.com. I want to be able to publish and preview post from my phone when i am away from my dev box. Idea is to have git webhook triggered on git push and deploy image to azure for preview.

## Running your jekyll site locally and deploying to Azure Container Service

Building and running container locally. Change docker-compose.yml file to insert your git repo url an change container name, branch, ports if needed.
> `docker-compose up -d --build`

Lookup logs locally and at this point you can preview your website locally
> `docker logs {LOCAL_CONTAINER_NAME} --follow`

Assuming you have your azure registry created this command will publish image. You don't need to rebuild image and publish image with every new blog post (git push)
> `docker push {YOUR_REGISTRY_FQDN}/jekyll-serve:latest`

Create container instance in azure
> `az container create --resource-group {YOUR_RESOURCE_GROUP} --name {YOUR_CONTAINER_NAME} --image {YOUR_REGISTRY_FQDN}/jekyll-serve --cpu 1 --memory 1 --registry-username {YOUR_USERNAME} --registry-password {YOUR_PASSWORD} --dns-name-label {YOUR_SUBDOMAIN} --ports 80`

See a staus of your container
> ` az container logs --resource-group {YOUR_RESOURCE_GROUP} --name {YOUR_CONTAINER_NAME} --follow`

Once testing is done you can delete container to free resources
> `az container delete --resource-group {YOUR_RESOURCE_GROUP} --name {YOUR_CONTAINER_NAME}`
 
 
 
 
 

## License

MIT License

Copyright (c) [2017] [George Trifonov]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.