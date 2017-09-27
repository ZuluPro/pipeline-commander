# pipeline-commander
A hackish tool to trigger a GitLab pipeline and wait for its completion.

# Usage
```
usage: pipeline-commander.py [-h] [-r GIT_REF] [-o TIMEOUT] [-i PROJECT_ID]
                             [-p PRIVATE_TOKEN] [-u SERVER_URL]
                             [-t TRIGGER_TOKEN] [-v [VERBOSE]]

Trigger a GitLab Pipeline and wait for its completion

optional arguments:
  -h, --help            show this help message and exit
  -r GIT_REF, --git-ref GIT_REF
                        Git reference, e.g. master
  -o TIMEOUT, --timeout TIMEOUT
                        Timeout in seconds
  -i PROJECT_ID, --project-id PROJECT_ID
                        Numerical Project ID (under Settings->General Project
                        Settings in GitLab
  -p PRIVATE_TOKEN, --private-token PRIVATE_TOKEN
                        An private or personal token authorised to query
                        pipeline status. See
                        https://docs.gitlab.com/ee/api/README.html#private-
                        tokens. By default, this value is initialized with
                        PRIVATE_TOKEN environment variable.
  -u SERVER_URL, --server-url SERVER_URL
                        Server URL to use, e.g. http://localhost:80
  -t TRIGGER_TOKEN, --trigger-token TRIGGER_TOKEN
                        The trigger token for a pipeline. See
                        https://docs.gitlab.com/ee/ci/triggers. By default,
                        this value is initialized with TRIGGER_TOKEN
                        environment variable.
  -v [VERBOSE], --verbose [VERBOSE]
                        Print more verbose information
```

# Example

The typical use-case is below

```bash
curl -s -o pipeline-commander.py https://goo.gl/146MEQ
python3 pipeline-commander.py \
	--server-url https://example.co \
  --project-id 11 \
  --git-ref master \
  --private-token AbcDefGhijkLmNopQrsT
  --trigger-token abcdef0123456789abcdef01234567
echo "exit status was $?"
```
Output:
```
================================================================================
http://example.co/namespace/project-name/pipelines/42
================================================================================
status: pending
..status: running
...........status: success
exit status was 0
```

For those who would like to avoid explicitly stating secret variables, the private token can also be specified using the environment variable `PRIVATE_TOKEN`. Similarly, the trigger token can also be specified using the environment variable `TRIGGER_TOKEN`.

Most continuous integration environments allow developers to specify secret variables that are cryptographically stored and sanitized in log files, but a trivial example of using environment variables is below.

```bash
export PRIVATE_TOKEN=AbcDefGhijkLmNopQrsT
export TRIGGER_TOKEN=abcdef0123456789abcdef01234567
curl -s -o pipeline-commander.py https://goo.gl/146MEQ
python3 pipeline-commander.py \
	--server-url https://example.co \
  --project-id 11 \
  --git-ref master
echo "exit status was $?"
```
Output:
```
================================================================================
http://example.co/namespace/project-name/pipelines/42
================================================================================
status: pending
..status: running
...........status: success
exit status was 0
```