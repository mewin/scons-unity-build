# Scons Unity Build Generator

Provides several generators for SCons to combine multiple source files into a bigger
one to reduce compilation time, so called "unity builds". This is achieved by generating
unity source files which in term include the actual source files and compile them using
one of the existing SCons builders.

# Usage

In order to use this, just place it inside your `site_scons/site_tools` folder, enable it by
adding "unity_build" to the tools when constructing your Environment and replace invocations
of the Program/Library/SharedLibrary/StaticLibrary builders with their Unity... counterpart:

```python
env = Environment(tools = ['default', 'unity_build'])

source_files = ...

env.UnityProgram(
    target = 'my_program',
    source = source_files,
    ...
)
```

The tool will generate an amount of unity source files and invoke the Program builder on these,
forwarding any other arguments you passed.

# Other Options

You can control the behaviour of the builder using several Environment options:
```python
env['UNITY_CACHE_DIR'] = '.unity' # Directory where the unity sources are stored.
                                  # can be either a string or a Dir() node.
env['UNITY_MAX_SOURCES'] = 15     # Maximum number of source files per unity file.
env['UNITY_MIN_FILES'] = env.GetOption('num_jobs')
                                  # Minimum number of unity files to generate (if possible).
                                  # Defaults to the number of jobs passed to SCons.
env['UNITY_DISABLE'] = False      # Set to True to completely disable unity builds. The commands
                                  # will simply pass through their options to the regular builders.
```

Additionally any generator can be passed a `cache_dir` to overwrite the value from the Environment.

# License
WTFPL, see LICENSE.txt
