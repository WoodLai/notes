```
name: Deploy via SSH

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "10.20.0" # 设置 Node.js 版本

      - name: Install GitBook CLI
        run: |
          node -v
          npm install gitbook-plugin-todo --save
          npm install gitbook-cli -g

      - name: init GitBook
        run: |
          gitbook init

      - name: install GitBook
        run: |
          gitbook install

      - name: build GitBook
        run: |
          gitbook build --verbose

      - name: Setup SSH
        uses: webfactory/ssh-agent@v0.9.0
        with:
          ssh-private-key: ${{ secrets.UBUNTU }}

      - name: deploy
        run: |
          cd _book
          git config --global user.name "woodlai"
          git config --global user.email "laishouqi@gmail.com"
          git init
          git remote add origin git@github.com:woodlai/note.git
          git branch -m gitbook
          git add .
          git commit -m "deploy by action"
          git push origin gitbook --force
```
