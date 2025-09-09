#npm #semver
Given a version number MAJOR.MINOR.PATCH, increment the:
1. MAJOR version when you make incompatible API changes
2. MINOR version when you add functionality in a backward compatible manner
3. PATCH version when you make backward compatible bug fixes
![[Pasted image 20250620112652.png]]
### Advantages of SemVer
- You can keep track of every transition in the software development phase.
- Versioning can do the job of explaining the developers about what type of changes have taken place and the possible updates that should take place in the software.
- It helps to keep things clean and meaningful.
- It helps other people who might be using your project as a dependency

### Using semantic versioning to specify update types your package can accept
You can specify which update types your package can accept from dependencies in you package's `package.json` file.
For example, to specify acceptable version ranges up to 1.0.4, use the following:
- Patch releases: `1.0` or `1.0.x` or `~1.0.4`
- Minor releases: `1` or `1.x` or `^1.0.4`
- Major releases: `*` or `x`

