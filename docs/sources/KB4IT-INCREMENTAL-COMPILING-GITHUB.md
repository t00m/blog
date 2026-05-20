---
Author: Tomás Vírseda
Category: Post
Command: date, git
Date: 2026-01-11 13:30:00
DocType: How-to guide
Product: Github Actions, KB4IT
Tag: cache, compiling, incremental, vm, workflow
Topic: Automation, Development
---

# KB4IT incremental compiling in Github (experimental)

## Excerpt

These days, after several fixes, I was able to produce a reliable incremental compiling of my knowledge base repositories (or force a whole compilation of the whole KB, if the app meets some conditions).

To my initial surprise, it didn't work in Github.

## Github actions

In Github, when a new update is detected in the source directory, the Github action in charge ([update-blog.yml](https://github.com/t00m/blog/blob/main/.github/workflows/update-blog.yml)) of updating the website is executed.

GitHub Actions runners are ephemeral. Every workflow run starts on a fresh VM, so anything written to disk is lost unless you explicitly persist it somewhere.

## Solution

Now, instead using `.kb4it` directory under `$USER` directory, it uses the own Github repository.
Then, once kb4it finishes the compilation, all changes are pushed automatically.

```yaml
      # Commit changes
      - name: Commit and push changes (if any)
        run: |
          git config --global user.name 'github-actions[bot]'
          git config --global user.email 'github-actions[bot]@users.noreply.github.com'

          # Stage all changes in docs/
          git add docs/
          git add var/

          # Check if there are changes to commit
          if git diff --staged --quiet; then
            echo "No changes to commit."
          else
            git commit -m "Auto-update target/ after KB4IT build [$(date -u)]"
            git push
          fi
```
