# VHS Action

GitHub Action to run [VHS][vhs].

<img alt="Welcome to VHS!" src="vhs.gif" width="600" />

[vhs]: https://github.com/charmbracelet/vhs

## Inputs

<table>

<thead>
  <tr>
  <th>Name</th>
  <th>Description</th>
  <th>Default</th>
  </tr>
</thead>

<tbody>
<tr>
<td>path</td>
<td>Path of the VHS `.tape` file</td>
<td>

```
"vhs.tape"
```

</td>
</tr>
<tr>
<td>version</td>
<td>Version of VHS to use</td>
<td>

```
"latest"
```
 
</td>
</tr>
<tr>
<td>token</td>
<td>GitHub token to use</td>
<td>

```
"latest"
```
 
</td>
</tr>
<tr>
<td>install-fonts</td>
<td>Whether to install fonts</td>
<td>

```
"true"
```
 
</td>
</tr>

</tbody>
</table>

## Example Action

The following is a workflow that uses VHS to generate new GIFs (from
 `vhs.tape`) and then auto commits the GIF file to the repository.

```yaml
name: vhs
on:
  push:
    paths:
      - vhs.tape
jobs:
  vhs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: charmbracelet/vhs-action@main
        with:
          path: 'vhs.tape'
      - uses: stefanzweifel/git-auto-commit-action@v4
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          commit_message: Update generated VHS GIF
          branch: main
          commit_user_name: vhs-action 📼
          commit_user_email: actions@github.com
          commit_author: vhs-action 📼 <actions@github.com>
          file_pattern: '*.gif'
```

Upload GIF to Imgur and comment in PR.

```yaml
name: comment gif
on:
  pull_request:
    paths:
      - vhs.tape
jobs:
  pr:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: charmbracelet/vhs-action@main
        with:
          path: 'vhs.tape'
      - uses: devicons/public-upload-to-imgur@v2.2.2
        id: imgur_step
        with:
          path: ./vhs.gif
          client_id: ${{ secrets.IMGUR_CLIENT_ID }} # Make sure you have this secret set in your repo
      - uses: github-actions-up-and-running/pr-comment@v1.0.1
        env:
          IMG_URL: ${{ fromJSON(steps.imgur_step.outputs.imgur_urls)[0] }}
          MESSAGE: |
            ![VHS GIf]({0})
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}
          message: ${{ format(env.MESSAGE, env.IMG_URL) }}

```

## Available Fonts

The action will provide the following fonts (and their [nerd font](nerdfonts)
variations) by default:

[nerdfonts]: https://www.nerdfonts.com

* Bitstream Vera Sans Mono
* DejaVu
* Fira Code
* Hack
* IBM Plex Mono
* JetBrains Mono
* Roboto Mono
* Source Code Pro
* Ubuntu Mono

## Feedback

We’d love to hear your thoughts on this project. Feel free to drop us a note!

* [Twitter](https://twitter.com/charmcli)
* [The Fediverse](https://mastodon.social/@charmcli)
* [Discord](https://charm.sh/chat)

## License

[MIT](https://github.com/charmbracelet/vhs/raw/main/LICENSE)

***

Part of [Charm](https://charm.sh).

<a href="https://charm.sh/">
  <img
    alt="The Charm logo"
    width="400"
    src="https://stuff.charm.sh/charm-badge.jpg"
  />
</a>

Charm热爱开源 • Charm loves open source
