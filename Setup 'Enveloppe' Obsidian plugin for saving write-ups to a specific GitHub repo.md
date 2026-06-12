- install `Enveloppe` community plugin
- in it's options, add GH username, repo name for write-ups, and branch (e.g. main).
- [create a PAT](https://github.com/settings/personal-access-tokens)(personal access token) for GH authentication to be able to do the uploads & interacting with the repo. *Note:*  A `fine-grained` PAT is more secure than a `classic` PAT, as it can limit its access to the specific write-ups repo used  (among many other benefits). After limiting it to the write-ups repo, select these permissions:
	- `Contents`: *Read and Write* (Required to create a branch and push files/images)
	- `Pull Requests`: *Read and Write* (Required because Enveloppe creates and merges PRs during the upload process)
	- `Metadata`: *Read-only* (Automatically selected; required for general GitHub API communication) 

>Enveloppe saves your API token locally inside your vault. To ensure you do not accidentally publish this secret key if you back up your entire vault to GitHub, make sure to add `.obsidian/plugins/obsidian-mkdocs-publisher/env` to your vault's `.gitignore` file.

- click the 'Test connection' button to confirm the plugin is working correctly
- can now start publishing write-ups. 
	- to achieve this, either set the key `share: true` in the frontmatter of a file, like this:
    
    ```
    ---
    share: true
    ---
    ```
    - or can toggle on "Share all files" under Plugin settings tab to publish without the share key set.
2. Now, run the command (via Ctrl-P or right-click) to publish: `Upload single current active note`
3. Then a PR will be created on the repo and automatically merged.
4. to change which folder in the Write-ups repo a file is uploaded to (instead of top-level), add `path` to the file's frontmatter:
```
---
share: true
path: "my-subfolder/my-note.md"
---
```