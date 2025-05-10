project('st-cmsis-l5', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.0.6',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-l5-1.0.6'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32l552__', 
  'stm32l562__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32l5xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32L5'  # This can be used with universal stm32 header 

  lib = static_library(
    f'st_cmsis_@type_lower@', system_src, startup_src,
    build_by_default: false,
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    c_args: type_define,
    cpp_args: type_define,
  )
  set_variable(f'st_cmsis_@type_lower@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    compile_args: [type_define, pkg_define],
    link_with: lib,
  ))
endforeach

# Per MCU dependencies with default linker script
mcus = {
    'stm32l552cc': ['stm32l552__', 256, 256],
    'stm32l552ce': ['stm32l552__', 512, 256],
    'stm32l552me': ['stm32l552__', 512, 256],
    'stm32l552qc': ['stm32l552__', 256, 256],
    'stm32l552qe': ['stm32l552__', 512, 256],
    'stm32l552rc': ['stm32l552__', 256, 256],
    'stm32l552re': ['stm32l552__', 512, 256],
    'stm32l552vc': ['stm32l552__', 256, 256],
    'stm32l552ve': ['stm32l552__', 512, 256],
    'stm32l552zc': ['stm32l552__', 256, 256],
    'stm32l552ze': ['stm32l552__', 512, 256],
    'stm32l562ce': ['stm32l562__', 512, 256],
    'stm32l562me': ['stm32l562__', 512, 256],
    'stm32l562qe': ['stm32l562__', 512, 256],
    'stm32l562re': ['stm32l562__', 512, 256],
    'stm32l562ve': ['stm32l562__', 512, 256],
    'stm32l562ze': ['stm32l562__', 512, 256],
}
foreach mcu, opts : mcus
  ld_default_script_name = f'@mcu@.ld'
  configure_file(input : 'default.ld.in',
    output : ld_default_script_name,
    configuration : {
      'flash_size_kb': opts[1],
      'ram_size_kb': opts[2],
  })
  ld_default_script = ['-Xlinker', '--default-script=' + meson.project_build_root() / ld_default_script_name]

  compile_args = []
  if get_option('use_hal')
    compile_args += '-DUSE_HAL_DRIVER'
  endif

  type_lower = opts[0].replace('_', 'x')
  set_variable(f'st_cmsis_@mcu@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: get_variable(f'st_cmsis_@type_lower@_dep'),
    compile_args: compile_args,
    link_args: ld_default_script,
  ))
endforeach