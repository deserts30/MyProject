# Triage Labels

`roblox-triage` speaks in canonical roles. This file maps them to the strings actually used in this project's tracker.

## Category roles

| Canonical      | This project |
| -------------- | ------------ |
| `bug`          | `bug`        |
| `enhancement`  | `enhancement`|

## State roles

| Canonical         | This project      | Meaning                                             |
| ----------------- | ----------------- | --------------------------------------------------- |
| `needs-triage`    | `needs-triage`    | Not yet evaluated                                   |
| `needs-info`      | `needs-info`      | Waiting on the user for a decision or detail        |
| `ready-to-build`  | `ready-to-build`  | Fully specified; an agent can build it in one pass  |
| `needs-human`     | `needs-human`     | Needs the user (Studio action, design/balance call) |
| `wontfix`         | `wontfix`         | Will not be actioned                                |

For GitHub, these are issue labels. Create them in your repository with:

```bash
gh label create "needs-triage" --color "d73a4a"
gh label create "needs-info" --color "0075ca"
gh label create "ready-to-build" --color "0e8a16"
gh label create "needs-human" --color "fbca04"
gh label create "wontfix" --color "ffffff"
```
