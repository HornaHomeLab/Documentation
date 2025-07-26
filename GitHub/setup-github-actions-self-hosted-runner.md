# Setup GitHub Actions Self-Hosted runner

A self-hosted runner is a system that you deploy and manage to execute jobs from GitHub Actions on GitHub. - [GitHub Docs](https://docs.github.com/en/actions/concepts/runners/self-hosted-runners)

GitHub Runner can be assigned to both entire GitHub organization or particular personal repository.
GitHub does not support assigning runner to the entire User Profile.

## Install Runner in the Operating System

GitHub Self hosted runner can be started on macOS, Linux and Windows.
To add a runner to the Organization follow the steps below:

1. Navigate to Organization Settings, `Actions -> Runners -> New runner`
2. Select the image for appropriate operating system and execute commands proposed by GitHub.
3. Set up a self-hosted runner as a service ([GitHub Docs](https://docs.github.com/en/actions/how-tos/manage-runners/self-hosted-runners/configure-the-application)):

```shell
sudo ./svc.sh install
sudo ./svc.sh start
sudo ./svc.sh status
```
