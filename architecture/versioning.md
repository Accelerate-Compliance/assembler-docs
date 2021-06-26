# Versioning method

The Assembler platform is built to enable application owners to version each application environment separately. 

New versions of the Assembler platform are released on a periodic basis. New versions are not automatically applied to individual environments and can be upgraded per application at time in which makes sense.

Assembler following [Semantic Versioning 2.0.0](https://semver.org/),

* MAJOR version when you make incompatible API changes,
* MINOR version when you add functionality in a backwards compatible manner, and
* PATCH version when you make backwards compatible bug fixes.

Alpha and Beta versions can also be release depending on functionality. 

Security patches are released for a variety of historic versions when they are required, and depending on the severity of the issue, may be automatically applied to applications.

When publishing from staging to production environments, the production environment must also be upgraded to match the staging version.