# GitHub App Token Orb

A CircleCI orb that generates short-lived GitHub App installation tokens for use in CI pipelines. Replaces long-lived personal access tokens with scoped, ephemeral tokens.

Forked from [ostk0069/github-app-token-orb](https://github.com/ostk0069/github-app-token-orb).

[![CircleCI Build Status](https://circleci.com/gh/Tyba-Energy/github-app-token-orb.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/Tyba-Energy/github-app-token-orb) [![CircleCI Orb Version](https://badges.circleci.com/orbs/tyba-energy/github-app-token.svg)](https://circleci.com/orbs/registry/orb/tyba-energy/github-app-token) [![GitHub License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](https://raw.githubusercontent.com/Tyba-Energy/github-app-token-orb/master/LICENSE)

## Usage

1. Create a GitHub App. Follow the instructions [here](https://docs.github.com/en/developers/apps/building-github-apps/authenticating-with-github-apps)
2. Get the App ID, Installation ID, and Private Key
3. Base64 encode your private key: `base64 -w 0 your-private-key.pem`
4. Store these as environment variables in a CircleCI context

## Example

```yml
version: '2.1'

orbs:
  github-app-token: tyba-energy/github-app-token@0.1.0

jobs:
  my-job:
    docker:
      - image: cimg/base:current
    steps:
      # Minimal — reads from GITHUB_APP_ID, GITHUB_APP_PRIVATE_KEY,
      # and GITHUB_APP_INSTALLATION_ID env vars
      - github-app-token/fetch-token

      # With explicit parameters and git URL rewriting
      - github-app-token/fetch-token:
          app_id: $MY_APP_ID
          base64_private_key: $MY_PRIVATE_KEY
          installation_id: $MY_INSTALLATION_ID
          rewrite_git_urls: true
```

## Parameters

| Parameter | Description | Required | Default | Type |
|---|---|---|---|---|
| app_id | ID of the GitHub App | No | `$GITHUB_APP_ID` | string |
| base64_private_key | Base64 encoded private key of the GitHub App | No | `$GITHUB_APP_PRIVATE_KEY` | string |
| installation_id | The ID of the installation for which the token will be requested | No | `$GITHUB_APP_INSTALLATION_ID` | string |
| env_name | Name of the environment variable to export the token as | No | `GITHUB_TOKEN` | string |
| repository_name | GitHub repository name (optional, scopes token to a single repo) | No | `""` | string |
| rewrite_git_urls | Configure git to rewrite GitHub SSH URLs to use HTTPS with the token | No | `false` | boolean |
| github_host | GitHub hostname (for GitHub Enterprise Server) | No | `github.com` | string |

## Changes from upstream

- `installation_id` changed from `integer` to `string` to allow env var references
- All parameters have sensible env var defaults — command works with zero args if env vars are set
- `env_name` defaults to `GITHUB_TOKEN` (was `GITHUB_APP_TOKEN`)
- Added `rewrite_git_urls` to configure git `insteadOf` rules for SSH-to-HTTPS rewriting
- Added `github_host` for GitHub Enterprise Server support

## Resources

[CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/tyba-energy/github-app-token) - The official registry page of this orb for all versions, executors, commands, and jobs described.

[CircleCI Orb Docs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) - Docs for using, creating, and publishing CircleCI Orbs.

### How to Contribute

We welcome [issues](https://github.com/Tyba-Energy/github-app-token-orb/issues) to and [pull requests](https://github.com/Tyba-Energy/github-app-token-orb/pulls) against this repository!
