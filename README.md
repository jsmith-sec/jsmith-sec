name: Generate Snake Animation

on:
  schedule:
    # runs once a day; regenerates the snake with your latest contributions
    - cron: "0 0 * * *"
  workflow_dispatch:      # lets you run it manually from the Actions tab
  push:
    branches:
      - main

jobs:
  generate:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Generate snake
        uses: Platane/snk@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake.svg?palette=github-dark&color_snake=FF3333&color_dots=1a1a1a,5c0000,990000,ff3333,ffffff

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
