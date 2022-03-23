# Verify Python version matches tag version

Common problem: tag a commit, have it release on PyPI, but you forgot
to update the version in the code!  This action verifies that the
following two things match, if not fail:
- The git tag version
- The `__version__` attribute from the module (module name inferred
  from `$PWD`)
