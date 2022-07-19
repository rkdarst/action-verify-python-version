# Verify Python version matches tag version

Common problem: tag a commit, have it release on PyPI, but you forgot
to update the version in the code!  This would result in a bad release
and should be detected and prevented.  This action verifies that the
following two things match, if not fail:
- The git tag version
- The `__version__` attribute from the module.

This would be used as part of a release workflow, like such:

```yaml
      - uses: rkdarst/action-verify-python-version@main
```

The module name is detected from of the repository directory name
(with `-` replaced with `_`) and must be importable.  If this isn't
correct, specify explicitly (this should be the name by which the
module can be imported):

```yaml
      - uses: rkdarst/action-verify-python-version@main
        with:
          module_name: my_python_module
```



## Assumptions and caveats

- The git tag is exactly the version
- The importable module name is the directory name (or specified with
  `module_name`)
- Version is found by `import MOD_NAME ; MOD_NAME.__version__`
- The package must be installable with `pip install .` and is
  installed as a byproduct of running this action.  This may be
  undesired in a few cases.



## Status

Used in a few projects, alpha quality.  Contributions welcome.
