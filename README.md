name: Metrics

on:
  schedule:
    - cron: "0 0 * * *"      # regenerate once a day
  workflow_dispatch:          # lets you trigger it manually from the Actions tab
  push:
    branches: ["main"]

jobs:
  # --- Stats + top languages, self-hosted (fixes the broken "GitHub Stats" card) ---
  github-metrics:
    runs-on: ubuntu-latest
    steps:
      - uses: lowlighter/metrics@latest
        with:
          filename: metrics.svg
          token: ${{ secrets.METRICS_TOKEN }}
          committer_token: ${{ secrets.GITHUB_TOKEN }}
          user: Jaermis
          template: classic
          base: header, activity, community, repositories, metadata
          config_timezone: Asia/Manila
          plugin_languages: yes
          plugin_languages_analysis_timeout: 15
          plugin_languages_categories: markup, programming
          plugin_languages_details: percentage

  # --- Trophy-style achievements, self-hosted (fixes the broken "Trophies" section) ---
  achievements:
    runs-on: ubuntu-latest
    steps:
      - uses: lowlighter/metrics@latest
        with:
          filename: achievements.svg
          token: ${{ secrets.METRICS_TOKEN }}
          committer_token: ${{ secrets.GITHUB_TOKEN }}
          user: Jaermis
          base: ""
          plugin_achievements: yes
          plugin_achievements_threshold: C
          plugin_achievements_secrets: yes
          plugin_achievements_display: compact

  # --- Commit activity graph, self-hosted (fixes the broken "My Contributions" activity line) ---
  activity:
    runs-on: ubuntu-latest
    steps:
      - uses: lowlighter/metrics@latest
        with:
          filename: activity.svg
          token: ${{ secrets.METRICS_TOKEN }}
          committer_token: ${{ secrets.GITHUB_TOKEN }}
          user: Jaermis
          base: ""
          plugin_isocalendar: yes
          plugin_isocalendar_duration: full-year
