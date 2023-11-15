# Facts

Git Large File Storage (LFS) is an extension to Git, the popular version control system, designed to improve the handling of large files. Here's a detailed explanation:

### Purpose
1. **Handle Large Files**: Git is optimized for source code, which generally involves small files. However, it's not efficient with large binary files (like videos, images, audio files, data sets), leading to slow clone and fetch operations.
2. **Avoid Repository Bloat**: Large files in a repository can significantly increase its size, making it cumbersome to manage.

### How it Works
1. **Pointer Files**: Instead of storing large files directly in the repository, Git LFS stores a pointer file in the repository. This pointer references the large file, which is stored separately.
2. **Separate Storage**: The actual large files are stored in a remote storage service (like GitHub LFS storage), not directly in the Git repository.
3. **Seamless Integration**: When you clone, pull, or push a repository using LFS, it automatically downloads and uploads large files as needed.

### Key Features
1. **Efficient Storage**: Reduces the size of the repository by storing large files externally.
2. **Version Control for Large Files**: Allows versioning for large files, similar to how Git manages code.
3. **Bandwidth Optimization**: Saves bandwidth by downloading large files on demand.

# How to Set Up Git LFS

### Setting Up Git LFS
1. **Installation**: First, install the Git LFS extension.
2. **Tracking Large Files**: Use `git lfs track` to specify which files (by extension or pattern) should be handled by LFS.
3. **Commit and Push**: Once tracked, these files are handled by LFS on commit and push operations.

### Practical Considerations
1. **Storage Quotas**: Be aware of storage limits and potential costs on the remote storage service.
2. **Compatibility**: Ensure that all collaborators have Git LFS installed for seamless operation.
3. **Suitability**: Best used for projects where large files are a necessity (e.g., game development, multimedia projects).

### Conclusion
Git LFS is a valuable tool for teams and projects dealing with large binary files, offering an efficient way to handle these files while leveraging Git's powerful version control capabilities. However, it's important to consider the additional setup and potential costs associated with using external storage services.

Implementing Git Large File Storage (LFS) involves a series of steps that you can execute using commands in the terminal. Here's a guide on how to set it up and use it:

### 1. Install Git LFS
First, you need to install the Git LFS extension. This is typically done using a package manager.

- **On macOS** (using Homebrew):
  ```bash
  brew install git-lfs
  ```

- **On Ubuntu/Debian**:
  ```bash
  sudo apt-get install git-lfs
  ```

- **On Windows**:
  Git LFS can be installed via a download from the [Git LFS website](https://git-lfs.github.com/), or if you have Chocolatey installed:
  ```bash
  choco install git-lfs
  ```

### 2. Initialize Git LFS in Your Repository
After installing, you need to set up Git LFS in your repository.

- Navigate to your Git repository:
  ```bash
  cd path/to/your/repo
  ```

- Initialize Git LFS:
  ```bash
  git lfs install
  ```

### 3. Track Large Files
Specify which files to track with LFS. This is usually done by file extension.

- For example, to track all `.zip` files:
  ```bash
  git lfs track "*.zip"
  ```

- After tracking files, a `.gitattributes` file is created/modified in your repository. This file should be committed:
  ```bash
  git add .gitattributes
  git commit -m "Track zip files with Git LFS"
  ```

### 4. Commit and Push
Now, when you commit and push files that match the tracked patterns, Git LFS will handle them automatically.

- Add your large file:
  ```bash
  git add your-large-file.zip
  git commit -m "Add large file"
  ```

- Push your changes:
  ```bash
  git push origin main
  ```

### 5. Cloning and Pulling LFS Files
When you clone a repository with LFS files, or pull changes that include LFS files, Git LFS will automatically download the large files.

- Clone a repository:
  ```bash
  git clone <repository-url>
  ```

- LFS files will be downloaded during the cloning process.

### 6. Checking LFS Files
You can check which files are being tracked by LFS or view LFS-related information.

- List tracked files:
  ```bash
  git lfs ls-files
  ```

- Show LFS file status:
  ```bash
  git lfs status
  ```

### Important Notes
- **Storage & Bandwidth**: Be aware of storage and bandwidth quotas of the remote server hosting your LFS files (like GitHub or Bitbucket).
- **Collaboration**: Ensure all collaborators have Git LFS installed and initialized to handle LFS files correctly.
- **Migration**: If you're adding LFS to an existing repository, you might need to migrate existing large files to LFS. This can be done using `git lfs migrate`.

Following these steps, you can efficiently manage large files in your Git repositories using Git LFS.

# Workflow with Github Desktop

Implementing Git Large File Storage (LFS) using GitHub Desktop is a straightforward process. GitHub Desktop is a graphical interface for Git that simplifies many Git tasks. Here's how you can use it to work with Git LFS:

### 1. Install Git LFS
First, you need to have Git LFS installed on your system. GitHub Desktop doesn't install Git LFS by default, so you need to do this manually.

- Download and install Git LFS from the [official website](https://git-lfs.github.com/).
- After installation, run `git lfs install` in your terminal to set it up.

### 2. Initialize a Repository with Git LFS
If you're starting a new repository:

- Create a new repository in GitHub Desktop.
- Open the repository in the terminal (there's an option in GitHub Desktop to open the repository in a command line).
- In the terminal, run `git lfs track` to track files. For example:
  ```bash
  git lfs track "*.psd"
  ```
- Commit the `.gitattributes` file that is created by the above command.

### 3. Adding Git LFS to an Existing Repository
If you're adding Git LFS to an existing repository:

- Open the repository in GitHub Desktop.
- Open the repository in the terminal through GitHub Desktop.
- In the terminal, run `git lfs track` to track the desired file types.
- Commit the changes to the `.gitattributes` file via GitHub Desktop.

### 4. Working with LFS-tracked Files
Once LFS is set up, you can work with large files just like any other file:

- Add your large files to the repository using GitHub Desktop as you normally would.
- Commit and push the changes. GitHub Desktop will use Git LFS automatically for files that match the tracking pattern.

### 5. Cloning and Pulling Repositories with LFS Files
- When cloning or pulling from a repository with LFS files, GitHub Desktop will handle the LFS files automatically.
- On the first clone/pull, you might see a prompt to initialize Git LFS, which you should accept.

### 6. Additional Information
- **Storage and Bandwidth Limits**: Be aware of the storage and bandwidth limits on GitHub for LFS files.
- **Collaborators**: Ensure collaborators working on the repository also have Git LFS installed.
- **Viewing LFS files**: GitHub Desktop does not display LFS files differently, but you can see which files are tracked by LFS by viewing the `.gitattributes` file.

Using GitHub Desktop with Git LFS simplifies the process of managing large files in your repositories, especially if you prefer a graphical interface over command-line operations.