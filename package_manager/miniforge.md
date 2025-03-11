# Managing your packages with Miniforge

When working on a project, you will need to install several packages to make it work. These packages can be installed using a package manager. In this guide, we will show you how to install Miniforge, a package manager that is compatible with Conda.

## Conda vs Mamba vs others

Conda is a package manager that is widely used in the Python community. It is a powerful tool that allows you to install, update, and manage packages in your Python environment. However, Conda can be slow at times, especially when installing large packages or resolving dependencies.

Mamba is a drop-in replacement for Conda that is designed to be faster and more efficient. It uses the same package index as Conda, so you can use it as a drop-in replacement for Conda without any changes to your workflow.

There are other package ad environment managers available, such as pip and venv, which are the default managers for Python. However, Conda and Mamba are more powerful, flexible and convinient (overall) than pip an venv, so they are preferred by many Python developers.

## Small vs Large

When working on serves where space is limited, smaller versions of the package managers are prefered. Miniconda is a small version of Conda that includes only the essential packages needed to get started. Miniforge is a small version of Mamba that includes only the essential packages needed to get started. The benefit of using Miniforge is that it is faster and more efficient than Conda, while still providing the same functionality and allowing <u>users familiar with Conda</u> to use it with almost no changes to their workflow.

    Note: After the release of Miniforge 23.3.1 in August 2023, Miniforge and Mambaforge are essentially identical. As of June 2024, Mambaforge is deprecated and was retired in January 2025. We recommend users switch to Miniforge3 immediately.

## Installing Miniforge

Miniforge can be easily installed in a Ubuntu (Linux) system by following these steps or by following the official documentation [here](https://github.com/conda-forge/miniforge):

1. First, be sure you have removed any previous installations of Conda or Miniconda. You can do this by running the following command:

```bash
rm -rf ~/miniconda
```
Further optional steps to completly remove Conda or Miniconda can be found [here](https://www.anaconda.com/docs/getting-started/miniconda/uninstall#uninstall-environments-outside-the-anaconda3-directory).

2. Download the Miniforge installer by using the following command:

```bash
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
```

3. Run the installer by using the following command:

```bash
bash Miniforge3-$(uname)-$(uname -m).sh
```

4. Follow the on-screen instructions to complete the installation. Done!

### Addional settings

When using Mamba for the first time, you may be asked to add some extra settings.

1. First, update mamba and conda to their latest versions:

```bash
mamba update mamba conda
```

2. Add Mamba root prefix as an environment variable (permanently):

```bash
echo 'export MAMBA_ROOT_PREFIX="$HOME/miniforge3"' >> ~/.bashrc
source ~/.bashrc
```
Essentially, this command adds a line to the `.bashrc` file, which makes the variable $MAMBA_ROOT_PREFIX available to all your terminal sessions.

3. Allow Mamba to initialize itself:

```bash
mamba shell init --shell bash
source ~/.bashrc
```

This line essentially modifies the .bashrc file to include the Mamba initialization script.


