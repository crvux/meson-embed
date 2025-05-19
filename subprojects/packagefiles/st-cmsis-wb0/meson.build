project('st-cmsis-wb0', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.2.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-wb0-1.2.0'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32wb05', 
  'stm32wb06', 
  'stm32wb07', 
  'stm32wb09', 
  'stm32wb0_', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32wb0xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32WB0'  # This can be used with universal stm32 header 

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
    'stm32wb05kn': 'stm32wb0_',
    'stm32wb05kz': 'stm32wb0_',
    'stm32wb05tn': 'stm32wb0_',
    'stm32wb05tz': 'stm32wb0_',
    'stm32wb06cc': 'stm32wb0_',
    'stm32wb06kc': 'stm32wb0_',
    'stm32wb07cc': 'stm32wb0_',
    'stm32wb07kc': 'stm32wb0_',
    'stm32wb09ke': 'stm32wb0_',
    'stm32wb09te': 'stm32wb0_',
}
foreach mcu, type : mcus
  set_variable(f'st_cmsis_@mcu@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: get_variable(f'st_cmsis_@type@_dep'),
    link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
  ))
endforeach