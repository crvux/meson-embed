project('st-cmsis-c0', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.3.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-c0-1.3.0'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32c011__', 
  'stm32c031__', 
  'stm32c051__', 
  'stm32c071__', 
  'stm32c091__', 
  'stm32c092__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32c0xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32C0'  # This can be used with universal stm32 header 

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
    'stm32c011d6': ['stm32c011__', 32, 6],
    'stm32c011f4': ['stm32c011__', 16, 6],
    'stm32c011f6': ['stm32c011__', 32, 6],
    'stm32c011j4': ['stm32c011__', 16, 6],
    'stm32c011j6': ['stm32c011__', 32, 6],
    'stm32c031c4': ['stm32c031__', 16, 12],
    'stm32c031c6': ['stm32c031__', 32, 12],
    'stm32c031f4': ['stm32c031__', 16, 12],
    'stm32c031f6': ['stm32c031__', 32, 12],
    'stm32c031g4': ['stm32c031__', 16, 12],
    'stm32c031g6': ['stm32c031__', 32, 12],
    'stm32c031k4': ['stm32c031__', 16, 12],
    'stm32c031k6': ['stm32c031__', 32, 12],
    'stm32c051c6': ['stm32c051__', 32, 12],
    'stm32c051c8': ['stm32c051__', 64, 12],
    'stm32c051f6': ['stm32c051__', 32, 12],
    'stm32c051f8': ['stm32c051__', 64, 12],
    'stm32c051g6': ['stm32c051__', 32, 12],
    'stm32c051g8': ['stm32c051__', 64, 12],
    'stm32c051k6': ['stm32c051__', 32, 12],
    'stm32c051k8': ['stm32c051__', 64, 12],
    'stm32c071c8': ['stm32c071__', 64, 24],
    'stm32c071cb': ['stm32c071__', 128, 24],
    'stm32c071f8': ['stm32c071__', 64, 24],
    'stm32c071fb': ['stm32c071__', 128, 24],
    'stm32c071g8': ['stm32c071__', 64, 24],
    'stm32c071gb': ['stm32c071__', 128, 24],
    'stm32c071k8': ['stm32c071__', 64, 24],
    'stm32c071kb': ['stm32c071__', 128, 24],
    'stm32c071r8': ['stm32c071__', 64, 24],
    'stm32c071rb': ['stm32c071__', 128, 24],
    'stm32c091cb': ['stm32c091__', 128, 36],
    'stm32c091cc': ['stm32c091__', 256, 36],
    'stm32c091fb': ['stm32c091__', 128, 36],
    'stm32c091fc': ['stm32c091__', 256, 36],
    'stm32c091gb': ['stm32c091__', 128, 36],
    'stm32c091gc': ['stm32c091__', 256, 36],
    'stm32c091kb': ['stm32c091__', 128, 36],
    'stm32c091kc': ['stm32c091__', 256, 36],
    'stm32c091rb': ['stm32c091__', 128, 36],
    'stm32c091rc': ['stm32c091__', 256, 36],
    'stm32c092cb': ['stm32c092__', 128, 30],
    'stm32c092cc': ['stm32c092__', 256, 30],
    'stm32c092fb': ['stm32c092__', 128, 30],
    'stm32c092fc': ['stm32c092__', 256, 30],
    'stm32c092gb': ['stm32c092__', 128, 30],
    'stm32c092gc': ['stm32c092__', 256, 30],
    'stm32c092kb': ['stm32c092__', 128, 30],
    'stm32c092kc': ['stm32c092__', 256, 30],
    'stm32c092rb': ['stm32c092__', 128, 30],
    'stm32c092rc': ['stm32c092__', 256, 30],
    'stm32c051d8': ['stm32c051__', 64, 12],
    'stm32c092ec': ['stm32c092__', 256, 30],
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