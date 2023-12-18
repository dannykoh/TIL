# 10 Use-Cases of gitattribute

With .gitattributes you can set some properties of git based on path and filename. It offers a lot of flexibility to influence various Git operations.

The file can be included in any directory. The rules inside the file will be matched to all files in the directory, including sub-directories.

## 10 examples
1. **Specify line endings:** Enforce LF or CRLF line endings for specific file types in your repository. This can be useful when collaborating across different OS. *.txt eol=lf (all txt files will use LF line endings)

2. **Marking files as binary:** Git will treat files as binary and won’t attempt to merge or diff them. *.jpg binary (JPG files will be treated as binary)

3. **Marking files as export-ignore:** Exclude files from archives (like ZIP or TAR files) created with git archive.
README.md export-ignore (README.md will be excluded from archives)

4. **Defining a diff driver:** Define custom diff drivers for different file types, which can be useful for non-text files.
*.docx diff=word (Use Word diff for DOCX files)

5. **Setting the merge driver:** Define how files are merged.
*.yml merge=yaml (Use YAML merge strategy for YAML files)

6. **Marking a file as locked:** Prevents users from pushing changes where multiple people have edited the same file in the same commit. *.lock lock (Lock files can't be edited simultaneously)

7. **Enabling/Disabling filters:** Apply or ignore certain filters, such as smudge/clean filters. *.txt filter=off (Disables filters for txt files)

8. **Setting language in GitHub linguist:** Control syntax highlighting and repository language statistics on GitHub.

9. **Ignore whitespace changes:** Ignore changes to whitespace in diff calculations. *.md -whitespace (Ignores whitespace changes in Markdown files)

10. **Defining file priority:** Define which file Git should prioritize if there's a conflict. *.txt priority=100 (Prioritizes txt files in the event of a conflict)