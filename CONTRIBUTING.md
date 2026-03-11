# Contributing to Ansible eduVPN Deployment

Thank you for considering contributing to the Ansible configuration for eduVPN! We're excited to have you. This guide will help you get started with the contribution process.

## Table of Contents

- [Contributing to Ansible eduVPN Deployment](#contributing-to-ansible-eduvpn-deployment)
  - [Table of Contents](#table-of-contents)
  - [How to Contribute](#how-to-contribute)
    - [Reporting Bugs](#reporting-bugs)
    - [Suggesting Features](#suggesting-features)
    - [Submitting Code Changes](#submitting-code-changes)
  - [Development Environment Setup](#development-environment-setup)
    - [Prerequisites](#prerequisites)
    - [Testing Changes locally](#testing-changes-locally)
  - [Style Guide](#style-guide)
  - [Handling Pull Requests from GitHub](#handling-pull-requests-from-github)

## How to Contribute

### Reporting Bugs

If you find a bug in the project, please open an issue with detailed information. Include steps to reproduce the issue, your environment details, and any relevant logs or outputs from your Ansible execution.

### Suggesting Features

We welcome feature suggestions! To suggest a new feature, please open an issue and describe your idea. Explain why the feature would be useful and how it should work.

### Submitting Code Changes

1. **Clone the Repository**:

    ```sh
    git clone https://github.com/ConsortiumGARR/ansible-eduvpn
    cd ansible-eduvpn
    ```

2. **Create a Branch**:

    ```sh
    git checkout -b feature/your-feature-name
    ```

3. **Make Your Changes**: Implement your changes in the new branch in the `ansible/` folder.
4. **Test Your Changes**: Ensure you test your Ansible playbooks against a local or staging Debian/Ubuntu server before committing.
5. **Commit Your Changes**:

    ```sh
    git add .
    git commit -m "Add feature: your feature name"
    ```

6. **Push to Your Branch**:

    ```sh
    git push origin feature/your-feature-name
    ```

7. **Open a Merge Request**: Go to the original repository and open a pull request. Provide a clear description of your changes and reference any related issues.

## Development Environment Setup

### Prerequisites

Ensure you have the following installed on your control machine:

- Python 3.8+
- Ansible ≥ 8.0

And a target Debian/Ubuntu server to test against with SSH access.

### Testing Changes locally

1. **Install Ansible Dependencies**:

    ```sh
    cd ansible/
    pip install -r requirements.txt
    ansible-galaxy collection install -r requirements.yml
    ```

2. **Configure local variables**:

    Set up your own `inventories/single/hosts.ini`, `group_vars/all/vars.yml` (replace placeholder values for `eduvpn_fqdn`, `server_ipv4`), and generate new local secrets using `ansible-vault` for `group_vars/all/vault.yml`.

3. **Run testing deployment**:

    ```sh
    ansible-playbook playbook.yml --ask-vault-pass
    ```

## Style Guide

Please try to follow `ansible-lint` best practices for formatting your YAML playbooks, tasks, and configurations. Try to keep formatting consistent with existing structure and provide documentation using the `name` attribute in every Ansible task block.

## Handling Pull Requests from GitHub

Our main repository is hosted on **GitLab** and is configured to **mirror pushes to GitHub**.
If someone opens a **Pull Request (PR) on GitHub**, it **must be merged through GitLab** to keep the repository history consistent.

1. **Fetch the PR locally from GitHub**

   ```bash
   git remote add github https://github.com/ConsortiumGARR/ansible-eduvpn.git  # only the first time
   git fetch github pull/<PR_NUMBER>/head:pr-<PR_NUMBER>
   ```

2. **Push the PR branch to GitLab**

   ```bash
   git checkout pr-<PR_NUMBER>
   git push origin pr-<PR_NUMBER>
   ```

3. **Open a Merge Request on GitLab**

   - Go to GitLab’s web interface.
   - Create a Merge Request from `pr-<PR_NUMBER>` into `main`.
   - Review and approve it as usual.

4. **Merge into `main` on GitLab**

   - Once the MR is merged, GitLab will automatically push the updated `main` branch to GitHub.

5. **Close the GitHub PR**

   - The GitHub PR will be automatically closed if the commits match.
   - Otherwise, close it manually with a note that it was merged via GitLab.
