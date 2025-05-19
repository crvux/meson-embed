project('st-cmsis-u3', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.1.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-u3-1.1.0'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32u375__', 
  'stm32u385__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32u3xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32U3'  # This can be used with universal stm32 header 

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
    'stm32u375ce': 'stm32u375__',
    'stm32u375cg': 'stm32u375__',
    'stm32u375ke': 'stm32u375__',
    'stm32u375kg': 'stm32u375__',
    'stm32u375re': 'stm32u375__',
    'stm32u375rg': 'stm32u375__',
    'stm32u375ve': 'stm32u375__',
    'stm32u375vg': 'stm32u375__',
    'stm32u385cg': 'stm32u385__',
    'stm32u385kg': 'stm32u385__',
    'stm32u385rg': 'stm32u385__',
    'stm32u385vg': 'stm32u385__',
}
foreach mcu, type : mcus
  set_variable(f'st_cmsis_@mcu@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: get_variable(f'st_cmsis_@type@_dep'),
    link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
  ))
endforeach