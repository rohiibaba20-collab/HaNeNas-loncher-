# HaNeNas-lonname: Build APK from zip
on:
  push:
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Zip kholo
        run: |
          mkdir -p /tmp/proj
          unzip -q "$(ls *.zip | head -n1)" -d /tmp/proj
          echo "DIR=$(dirname "$(find /tmp/proj -name settings.gradle.kts | head -n1)")" >> $GITHUB_ENV
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.7
      - name: APK banao
        working-directory: ${{ env.DIR }}
        run: gradle assembleDebug --no-daemon
      - uses: actions/upload-artifact@v4
        with:
          name: HaNeNaLauncher-APK
          path: ${{ env.DIR }}/app/build/outputs/apk/debug/*.apkcher-
