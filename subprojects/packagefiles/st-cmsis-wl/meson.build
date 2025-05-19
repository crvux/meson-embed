project('st-cmsis-wl', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.2.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-wl-1.2.0'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32wl54__', 
  'stm32wl55__', 
  'stm32wl5m__', 
  'stm32wle4__', 
  'stm32wle5__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32wlxx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32WL'  # This can be used with universal stm32 header 

  lib = static_library(
    f'st_cmsis_@type_lower@', system_src, startup_src,
    build_by_default: false,
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    c_args: type_define,
    cpp_args: type_define,
  )
  set_variable(f'st_cmsis_@type@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    compile_args: [type_define, pkg_define],
    link_with: lib,
  ))
endforeach

# Per MCU dependencies with default linker script
mcus = {
    'stm32wl54cc': 'stm32wl54__',
    'stm32wl54jc': 'stm32wl54__',
    'stm32wl55cc': 'stm32wl55__',
    'stm32wl55jc': 'stm32wl55__',
    'stm32wl5moc': 'stm32wl5m__',
    'stm32wle4c8': 'stm32wle4__',
    'stm32wle4cb': 'stm32wle4__',
    'stm32wle4cc': 'stm32wle4__',
    'stm32wle4j8': 'stm32wle4__',
    'stm32wle4jb': 'stm32wle4__',
    'stm32wle4jc': 'stm32wle4__',
    'stm32wle5c8': 'stm32wle5__',
    'stm32wle5cb': 'stm32wle5__',
    'stm32wle5cc': 'stm32wle5__',
    'stm32wle5j8': 'stm32wle5__',
    'stm32wle5jb': 'stm32wle5__',
    'stm32wle5jc': 'stm32wle5__',
}
foreach mcu, type : mcus
  set_variable(f'st_cmsis_@mcu@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: get_variable(f'st_cmsis_@type@_dep'),
    link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
  ))
endforeach