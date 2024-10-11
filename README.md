# Setup Pharo

<p>
  <a href="https://github.com/features/actions">
    <img src="https://img.shields.io/badge/GitHub-Action-blue?logo=github" alt="GitHub Action" />
  </a>
  <a href="https://help.github.com/en/actions/automating-your-workflow-with-github-actions/virtual-environments-for-github-hosted-runners#supported-runners-and-hardware-resources">
    <img src="https://img.shields.io/badge/platform-macOS%20%7C%20Ubuntu%20%7C%20Windows-lightgray" alt="Supports macOS, Ubuntu & Windows" />
  </a>
</p>

[GitHub Action](https://github.com/features/actions) that will setup a [Pharo](https://pharo.org) environment with a specific version. Works on Ubuntu, macOS and Windows runners.

It will install the Pharo Virtual Machine as well as the Pharo image of the version specified as parameter.
It will also add some environment variables to ease the use of Pharo.
The command to run Pharo will be available in the `PHARO` environment variable.

## Usage

To install and use a Pharo 13 image, 
```yaml
    steps:
      - uses: demarey/pharo-setup-gha@main
        with:
          version: 13

      - name: Run Pharo
        run: |
          $PHARO --headless Pharo.image eval 'SystemVersion current printString'
```

This action supports Windows runners and you can take advantage of it to define jobs that will run on many OS. In this case, you will need to use bash and tell the plugin that you want to use Pharo from bash.
In the followin example, you will test on ubuntu, mac Os and Windows 3 different Pharo versions: 11, 12 and 13.
```yaml
jobs:
  build:
    strategy:
      fail-fast: false
      matrix:
        version: [11, 12, 13]
        os: [ubuntu-latest, macos-latest, windows-latest]

    runs-on: ${{ matrix.os }}
    
    steps:
      - uses: demarey/pharo-setup-gha@main
        with:
          version: ${{ matrix.version }}
          useBashOnWindows: true

      - name: Test installation
        run: |
          $PHARO --headless Pharo.image eval 'SystemVersion current printString'
        shell: bash
```

You can also specify the directory where the VM or the image will be stored:
```yaml
- uses: demarey/pharo-setup-gha@main
  with:
    version: 13
    imageDir: myImage
    vmDir: foo/vm
```

It is also possible to set the image name with the `imageName` parameter.

## Legal
Uses MIT license. 
