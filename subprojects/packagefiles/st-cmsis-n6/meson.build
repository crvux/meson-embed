project('st-cmsis-n6', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.1.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-n6-1.1.0'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32n645__', 
  'stm32n647__', 
  'stm32n655__', 
  'stm32n657__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32n6xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32N6'  # This can be used with universal stm32 header 

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
    'stm32n645a0': 'stm32n645__',
    'stm32n645b0': 'stm32n645__',
    'stm32n645i0': 'stm32n645__',
    'stm32n645l0': 'stm32n645__',
    'stm32n645x0': 'stm32n645__',
    'stm32n645z0': 'stm32n645__',
    'stm32n647a0': 'stm32n647__',
    'stm32n647b0': 'stm32n647__',
    'stm32n647i0': 'stm32n647__',
    'stm32n647l0': 'stm32n647__',
    'stm32n647x0': 'stm32n647__',
    'stm32n647z0': 'stm32n647__',
    'stm32n655a0': 'stm32n655__',
    'stm32n655b0': 'stm32n655__',
    'stm32n655i0': 'stm32n655__',
    'stm32n655l0': 'stm32n655__',
    'stm32n655x0': 'stm32n655__',
    'stm32n655z0': 'stm32n655__',
    'stm32n657a0': 'stm32n657__',
    'stm32n657b0': 'stm32n657__',
    'stm32n657i0': 'stm32n657__',
    'stm32n657l0': 'stm32n657__',
    'stm32n657x0': 'stm32n657__',
    'stm32n657z0': 'stm32n657__',
}
foreach mcu, type : mcus
  set_variable(f'st_cmsis_@mcu@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: get_variable(f'st_cmsis_@type@_dep'),
    link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
  ))
endforeach