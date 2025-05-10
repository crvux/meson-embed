project('st-cmsis-wb', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.12.2',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-wb-1.12.2'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32wb55__', 
  'stm32wb5m__', 
  'stm32wb50__', 
  'stm32wb35__', 
  'stm32wb30__', 
  'stm32wb15__', 
  'stm32wb10__', 
  'stm32wb1m__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32wbxx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32WB'  # This can be used with universal stm32 header 

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
    'stm32wb10cc': ['stm32wb10__', 320, 48],
    'stm32wb15cc': ['stm32wb15__', 320, 48],
    'stm32wb1mmc': ['stm32wb1m__', 320, 48],
    'stm32wb30ce': ['stm32wb30__', 512, 96],
    'stm32wb35cc': ['stm32wb35__', 256, 96],
    'stm32wb35ce': ['stm32wb35__', 512, 96],
    'stm32wb50cg': ['stm32wb50__', 1024, 128],
    'stm32wb55cc': ['stm32wb55__', 256, 128],
    'stm32wb55ce': ['stm32wb55__', 512, 256],
    'stm32wb55cg': ['stm32wb55__', 1024, 256],
    'stm32wb55rc': ['stm32wb55__', 256, 128],
    'stm32wb55re': ['stm32wb55__', 512, 256],
    'stm32wb55rg': ['stm32wb55__', 1024, 256],
    'stm32wb55vc': ['stm32wb55__', 256, 128],
    'stm32wb55ve': ['stm32wb55__', 512, 256],
    'stm32wb55vg': ['stm32wb55__', 1024, 256],
    'stm32wb55vy': ['stm32wb55__', 640, 256],
    'stm32wb5mmg': ['stm32wb5m__', 1024, 256],
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