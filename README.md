# Basic auth token action

## What is it?

A simple action to set correctly the authentication token in an action.

## How to use it?

```yaml
- name: set up github creds
  uses: y-nk/basic-auth-token-action@main
  with:
    username: ${{ github.actor }}
    password: ${{ github.token }}
```

```yaml
- name: set up other user github creds
  uses: y-nk/basic-auth-token-action@main
  with:
    username: my-other-user
    password: ${{ secrets.OTHER_USER_GH_PAT }}
```

## Why should I use it?

Several edge cases could be a reason:

1. You have a bot to make some git push (like automated releases) and you want to use that account instead. In that case you should have a Personal Access Token for that bot and write permissions to the repo.

2. For some reason you are caching the whole repo with `actions/cache` and want to make further git action in other jobs reusing that cache. This is very niche, but still. When doing so, the first step of caching the whole repo will include `.git` and its local config, which already has a `http.https://github.com/.extraheader` value matching the current runner's valid token ; that said the token is invalidated after run, so when you restore the cache from `actions/cache`, you'll restore with the `.git` and its config, and the token won't be accepted anymore.
