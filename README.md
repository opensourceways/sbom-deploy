# sbom-service


## Run in Docker

**Replace *BRANCH* in Dockerfile with specific branch**

Database configures, such as *host/port/db_name* are passed to the container via
environment variables.

Sensitive data, such as *password*, are passed by *docker secret*.

1. `docker swarm init`
2. `printf {your_db_user_password} | docker secret create db_password -`
3. `printf {your_ossindex_api_token} | docker secret create ossindex_api_token -`
4. `printf {your_github_api_token} | docker secret create github_api_token -`
5. `printf {your_gitee_api_token} | docker secret create gitee_api_token -`
6. `printf {your_gitlab_api_token} | docker secret create gitlab_api_token -`
7. `docker build . -t sbom-service`
8. ```
    docker service create 
    --secret="db_password" \
    --secret="ossindex_api_token" \
    --secret="github_api_token" \
    --secret="gitee_api_token" \
    --secret="gitlab_api_token" \
    --publish published={your_host_port},target={your_container_port} \
    -e DB_HOST="{your_db_host}" \
    -e DB_PORT="{your_db_port}" \
    -e DB_NAME="{your_db_name}" \
    -e DB_USERNAME="{your_db_username}" \
    -e DB_PASSWORD_FILE="/run/secrets/db_password" \
    -e OSSINDEX_API_TOKEN_FILE="/run/secrets/ossindex_api_token" \
    -e GITHUB_API_TOKEN_FILE="/run/secrets/github_api_token" \
    -e GITEE_API_TOKEN_FILE="/run/secrets/gitee_api_token" \
    -e GITLAB_API_TOKEN_FILE="/run/secrets/gitlab_api_token" \
    sbom-service
    ```

# sbom-website

**Replace *BRANCH* in Dockerfile with specific branch**

1. `docker build -t sbom-website .`
2. `docker run -d -p 8080:8080 sbom-website`