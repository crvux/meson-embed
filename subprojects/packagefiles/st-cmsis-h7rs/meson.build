project('st-cmsis-h7rs', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.2.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-h7rs-1.2.0'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32h7r3__', 
  'stm32h7r7__', 
  'stm32h7s3__', 
  'stm32h7s7__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32h7rsxx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32H7RS'  # This can be used with universal stm32 header 

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
    'stm32h7r3a8': ['stm32h7r3__', 64, 620],
    'stm32h7r3i8': ['stm32h7r3__', 64, 620],
    'stm32h7r3l8': ['stm32h7r3__', 64, 620],
    'stm32h7r3r8': ['stm32h7r3__', 64, 620],
    'stm32h7r3v8': ['stm32h7r3__', 64, 620],
    'stm32h7r3z8': ['stm32h7r3__', 64, 620],
    'stm32h7r7a8': ['stm32h7r7__', 64, 620],
    'stm32h7r7i8': ['stm32h7r7__', 64, 620],
    'stm32h7r7l8': ['stm32h7r7__', 64, 620],
    'stm32h7r7z8': ['stm32h7r7__', 64, 620],
    'stm32h7s3a8': ['stm32h7s3__', 64, 620],
    'stm32h7s3i8': ['stm32h7s3__', 64, 620],
    'stm32h7s3l8': ['stm32h7s3__', 64, 620],
    'stm32h7s3r8': ['stm32h7s3__', 64, 620],
    'stm32h7s3v8': ['stm32h7s3__', 64, 620],
    'stm32h7s3z8': ['stm32h7s3__', 64, 620],
    'stm32h7s7a8': ['stm32h7s7__', 64, 620],
    'stm32h7s7i8': ['stm32h7s7__', 64, 620],
    'stm32h7s7l8': ['stm32h7s7__', 64, 620],
    'stm32h7s7z8': ['stm32h7s7__', 64, 620],
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