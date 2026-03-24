# Different Branch CI/CD Demo

This project demonstrates a Jenkins pipeline that handles multiple environments through a single configuration file, identifying branches and deploying to the appropriate servers.

## Project Structure

- `app.js`: Simple Express server.
- `package.json`: Project dependencies and metadata.
- `Dockerfile`: Containerization setup for the application.
- `Jenkinsfile`: CI/CD pipeline definition for multi-branch deployments.

## Branching & Deployment Strategy

The project utilizes a 4-branch model to manage different stages of the application lifecycle:

| Branch | Environment | Pipeline Action | Deployment Port |
| :--- | :--- | :--- | :--- |
| `prod` | Production | **Triggered** - Deploys to Production | 3003 |
| `dev` | Development | **Triggered** - Deploys to Development | 3004 |
| `stg` | Staging | **No Pipeline** - Manual Audit | N/A |
| `uat` | UAT | **No Pipeline** - Manual Audit | N/A |

### CI/CD Workflow (Jenkins)

- **Automatic Deployments**:
    - Pushing to the `dev` branch triggers a build and deployment to the Development server on port 3004.
    - Pushing to the `prod` branch triggers a build and deployment to the Production server on port 3003.
- **Manual Stages**:
    - The `stg` and `uat` branches are tracked in the repository for staging and user acceptance testing but do not have automated deployment triggers in the current `Jenkinsfile`.

## How to Run

### Locally
1. Install dependencies: `npm install`
2. Start the server: `node app.js`

### Using Docker
1. Build the image: `docker build -t differentbranch .`
2. Run the container: `docker run -p 3000:3000 differentbranch`

## Jenkins Pipeline Details

The `Jenkinsfile` is configured to:
1. Support an `ubuntu-agent`.
2. Perform a source code checkout.
3. Build a Docker image tagged with the branch name.
4. Execute environment-specific deployment logic based on the `BRANCH_NAME` variable.
