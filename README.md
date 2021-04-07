# Github Actions
## Github Actions For Flutter


## Getting Started

Goto [Actions](https://github.com/kapilmhr/flutter_github_actions/actions) in top Bar.

Click on latest workflow from the list.

Find the latest APK.


## Create a workflow file

- Goto  project and manually create **.github** folder
- Create subfolder inside it with name **workflows**
- Create a file with extension **.yml**
- **For example**  .github/workflows/flutter-ci.yml


## Working on Workflow File 

### `on`
Github actions will work execute workflow following the events under **On**

```
# This workflow is triggered on pushes to the repository.

on:
  push:
    branches:
      - main

    # on: push    # Default will running for every branch.
```

### `job`
A workflow run is made up of one or more jobs.

### `runs-on`
The type of virtual host machine to run the job on. we will be using ubuntu - 20.04 version

```
jobs:
  build:
    # This job will run on ubuntu virtual machine
    runs-on: ubuntu-20.04
```

### `steps`
Steps are the sequence of tasks in a job.
1. Setup Java Environment for Android app
2. Setup Flutter Environment- using [subisito/flutter-actions](https://github.com/subosito/flutter-action)
```
    steps:

      # Setup Java environment in order to build the Android app.
      - uses: actions/checkout@v2
      - uses: actions/setup-java@v1
        with:
          java-version: '12.x'

      # Setup the flutter environment.
      - uses: subosito/flutter-action@v1
        with:
          channel: 'beta' # 'dev', 'alpha', default to: 'stable'
          # flutter-version: '1.12.x' # you can also specify exact version of flutter

```

3. Write the command to get the Flutter dependencies.
``` flutter pub get```
4. Check for any formatting issues in the code.
```flutter format --set-exit-if-changed```
5. Statically analyze the Dart code for any errors.
```flutter analyze```
6. Run widget tests for our flutter project.
```flutter test```
7. Build an Android APK.
```flutter build apk```

8. Finally, upload our generated app-release.apk for our workflow to the artifacts. For this, we will be using [actions/upload-artifact](https://github.com/actions/upload-artifact).
```
   - uses: actions/upload-artifact@v1
      with:
        # Name of the command/step.
        name: release-apk
        # Path to the release apk.
        path: build/app/outputs/apk/release/app-release.apk
```

## Watch your builds

Once you have pushed to your main branch, you can immediately see the build starting for your latest commit on GitHub under the Actions tab

<img src="https://github.com/kapilmhr/flutter_github_actions/blob/main/snapshots/build1.png" height="350">

### View Logs

<img src="https://github.com/kapilmhr/flutter_github_actions/blob/main/snapshots/build2.png" height="350">

