
# Jira Login

Used to store credentials for later use by other Jira Actions

> ##### Only supports Jira Cloud. Does not support Jira Server (hosted)

## Usage
An example workflow to create a Jira issue for each `//TODO` in code:

```yaml
on: push

name: Jira Example

jobs:
  build:
    runs-on: ubuntu-latest
    name: Jira Example
    steps:
    - name: Login
      uses: carlosnizolli/gajira-login@v01
      env:
        JIRA_BASE_URL: ${{ secrets.JIRA_BASE_URL }}
        JIRA_USER_EMAIL: ${{ secrets.JIRA_USER_EMAIL }}
        JIRA_API_TOKEN: ${{ secrets.JIRA_API_TOKEN }}

      - name: Find JIRA in Head Message
        uses: carlosnizolli/gajira-find-issue-key@v01
        with:
            string: ${{ github.event.workflow_run.head_commit.message }}

      - name: Find JIRA in Branch Name
        uses: carlosnizolli/gajira-find-issue-key@v01
        with:
            string: ${{ github.head_ref }}

```

More examples at [gajira](https://github.com/atlassian/gajira) repository

----
## Action Spec:

### Enviroment variables
- `JIRA_BASE_URL` - URL of Jira instance. Example: `https://<yourdomain>.atlassian.net`
- `JIRA_API_TOKEN` - **Access Token** for Authorization. Example: `HXe8DGg1iJd2AopzyxkFB7F2` ([How To](https://confluence.atlassian.com/cloud/api-tokens-938839638.html))
- `JIRA_USER_EMAIL` - email of the user for which **Access Token** was created for . Example: `human@example.com`

### Arguments
- None

### Writes fields to config file at $HOME/jira/config.yml
- `email` - user email
- `token` - api token
- `baseUrl` - URL for Jira instance

### Writes fields to CLI config file at $HOME/.jira.d/config.yml
- `endpoint` - URL for Jira instance
- `login` - user email

### Writes env to file at $HOME/.jira.d/credentials
- `JIRA_API_TOKEN` - Jira API token to use with CLI
