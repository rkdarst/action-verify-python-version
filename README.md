# Verify Python version matches tag version

Common problem: tag a commit, have it release on PyPI, but you forgot
to update the version in the code!  This would result in a bad release
and should be detected and prevented.  This action verifies that the
following two things match, if not fail:
- The git tag version (`refs/tags/VERSION`)
- The `MODULE.__version__` attribute from the module.

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


## Configuration

`with:` in the action:

* `module_name`: Use this as the python module name (it will detect
  the version via `module_name.__version__`) rather than
  auto-detecting the module name from the directory name.  Default:
  auto-detect.
* `tag_remove_prefix`: strip this from the start of tag name (if it's not
  there, succeed without stripping anything).  Default: `v`.
* `tag_remove_suffix`: strip this from the end of the tag name (if
  it's not there, succeed without stripping anything).  Default: `''`
  (don't strip anything)
* `no_install`: If this is set to anything, don't run `pip install -e
  .` before this action.  You must install the package yourself so
  that Python can import it and run `module_name.__version__`, if it's
  not already importable somehow.


## Assumptions and caveats

- The git tag is exactly the version (but see the configuration
  above).
- The importable module name is the directory name (or specified with
  `module_name`)
- Version is found by `import MODULE_NAME ; MODULE_NAME.__version__`
- The package must be installable with `pip install .` and is
  installed as a byproduct of running this action.  This may be
  undesired in a few cases (but this can be disabled).


## Status

Used in a few projects, alpha quality.  Contributions welcome.
