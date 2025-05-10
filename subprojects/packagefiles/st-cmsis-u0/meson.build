project('st-cmsis-u0', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.2.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-u0-1.2.0'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32u073__', 
  'stm32u083__', 
  'stm32u031__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32u0xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32U0'  # This can be used with universal stm32 header 

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
    'stm32u031c6': ['stm32u031__', 32, 12],
    'stm32u031c8': ['stm32u031__', 64, 12],
    'stm32u031f4': ['stm32u031__', 16, 12],
    'stm32u031f6': ['stm32u031__', 32, 12],
    'stm32u031f8': ['stm32u031__', 64, 12],
    'stm32u031k4': ['stm32u031__', 16, 12],
    'stm32u031k6': ['stm32u031__', 32, 12],
    'stm32u031k8': ['stm32u031__', 64, 12],
    'stm32u031r6': ['stm32u031__', 32, 12],
    'stm32u031r8': ['stm32u031__', 64, 12],
    'stm32u073c8': ['stm32u073__', 64, 40],
    'stm32u073cb': ['stm32u073__', 128, 40],
    'stm32u073cc': ['stm32u073__', 256, 40],
    'stm32u073k8': ['stm32u073__', 64, 40],
    'stm32u073kb': ['stm32u073__', 128, 40],
    'stm32u073kc': ['stm32u073__', 256, 40],
    'stm32u073m8': ['stm32u073__', 64, 40],
    'stm32u073mb': ['stm32u073__', 128, 40],
    'stm32u073mc': ['stm32u073__', 256, 40],
    'stm32u073r8': ['stm32u073__', 64, 40],
    'stm32u073rb': ['stm32u073__', 128, 40],
    'stm32u073rc': ['stm32u073__', 256, 40],
    'stm32u083cc': ['stm32u083__', 256, 40],
    'stm32u083kc': ['stm32u083__', 256, 40],
    'stm32u083mc': ['stm32u083__', 256, 40],
    'stm32u083rc': ['stm32u083__', 256, 40],
    'stm32u031g6': ['stm32u031__', 32, 12],
    'stm32u031g8': ['stm32u031__', 64, 12],
    'stm32u073h8': ['stm32u073__', 64, 40],
    'stm32u073hb': ['stm32u073__', 128, 40],
    'stm32u073hc': ['stm32u073__', 256, 40],
    'stm32u083hc': ['stm32u083__', 256, 40],
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