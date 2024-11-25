# Workflow Syntax YAML

---

**name**

- required: no

name of the workflow. if this field is omitted, the name of the workflow file will be used instead.

for example,

```yaml
name: learn-github-actions
```

---

**run-name**

- required: no

the name for workflow runs generated from the workflow, which will appear in the list of workflow runs on your repository's "Actions" tab.

for example,

```yaml
run-name: ${{ github.actor }} is learning GitHub Actions
```

---

**on**

- required: yes

specifies the trigger for this workflow.

for example,

```yaml
on: [push]
```

this example uses the `push` event, so a workflow run is triggered every time someone pushes a change to the repository or merges a pull request.

---

## References

- https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions
