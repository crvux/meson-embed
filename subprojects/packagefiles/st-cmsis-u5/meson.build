project('st-cmsis-u5', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.4.1',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-u5-1.4.1'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32u575__', 
  'stm32u585__', 
  'stm32u595__', 
  'stm32u599__', 
  'stm32u5a5__', 
  'stm32u5a9__', 
  'stm32u5f9__', 
  'stm32u5g9__', 
  'stm32u5f7__', 
  'stm32u5g7__', 
  'stm32u535__', 
  'stm32u545__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32u5xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32U5'  # This can be used with universal stm32 header 

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
    'stm32u535cb': ['stm32u535__', 128, 274],
    'stm32u535cc': ['stm32u535__', 256, 274],
    'stm32u535ce': ['stm32u535__', 512, 274],
    'stm32u535je': ['stm32u535__', 512, 274],
    'stm32u535nc': ['stm32u535__', 256, 274],
    'stm32u535ne': ['stm32u535__', 512, 274],
    'stm32u535rb': ['stm32u535__', 128, 274],
    'stm32u535rc': ['stm32u535__', 256, 274],
    'stm32u535re': ['stm32u535__', 512, 274],
    'stm32u535vc': ['stm32u535__', 256, 274],
    'stm32u535ve': ['stm32u535__', 512, 274],
    'stm32u545ce': ['stm32u545__', 512, 274],
    'stm32u545je': ['stm32u545__', 512, 274],
    'stm32u545ne': ['stm32u545__', 512, 274],
    'stm32u545re': ['stm32u545__', 512, 274],
    'stm32u545ve': ['stm32u545__', 512, 274],
    'stm32u575ag': ['stm32u575__', 1024, 786],
    'stm32u575ai': ['stm32u575__', 2048, 786],
    'stm32u575cg': ['stm32u575__', 1024, 786],
    'stm32u575ci': ['stm32u575__', 2048, 786],
    'stm32u575og': ['stm32u575__', 1024, 786],
    'stm32u575oi': ['stm32u575__', 2048, 786],
    'stm32u575qg': ['stm32u575__', 1024, 786],
    'stm32u575qi': ['stm32u575__', 2048, 786],
    'stm32u575rg': ['stm32u575__', 1024, 786],
    'stm32u575ri': ['stm32u575__', 2048, 786],
    'stm32u575vg': ['stm32u575__', 1024, 786],
    'stm32u575vi': ['stm32u575__', 2048, 786],
    'stm32u575zg': ['stm32u575__', 1024, 786],
    'stm32u575zi': ['stm32u575__', 2048, 786],
    'stm32u585ai': ['stm32u585__', 2048, 786],
    'stm32u585ci': ['stm32u585__', 2048, 786],
    'stm32u585oi': ['stm32u585__', 2048, 786],
    'stm32u585qi': ['stm32u585__', 2048, 786],
    'stm32u585ri': ['stm32u585__', 2048, 786],
    'stm32u585vi': ['stm32u585__', 2048, 786],
    'stm32u585zi': ['stm32u585__', 2048, 786],
    'stm32u595ai': ['stm32u595__', 2048, 2514],
    'stm32u595aj': ['stm32u595__', 4096, 2514],
    'stm32u595qi': ['stm32u595__', 2048, 2514],
    'stm32u595qj': ['stm32u595__', 4096, 2514],
    'stm32u595ri': ['stm32u595__', 2048, 2514],
    'stm32u595rj': ['stm32u595__', 4096, 2514],
    'stm32u595vi': ['stm32u595__', 2048, 2514],
    'stm32u595vj': ['stm32u595__', 4096, 2514],
    'stm32u595zi': ['stm32u595__', 2048, 2514],
    'stm32u595zj': ['stm32u595__', 4096, 2514],
    'stm32u599bj': ['stm32u599__', 4096, 2514],
    'stm32u599ni': ['stm32u599__', 2048, 2514],
    'stm32u599nj': ['stm32u599__', 4096, 2514],
    'stm32u599vi': ['stm32u599__', 2048, 2514],
    'stm32u599vj': ['stm32u599__', 4096, 2514],
    'stm32u599zi': ['stm32u599__', 2048, 2514],
    'stm32u599zj': ['stm32u599__', 4096, 2514],
    'stm32u5a5aj': ['stm32u5a5__', 4096, 2514],
    'stm32u5a5qi': ['stm32u5a5__', 2048, 2514],
    'stm32u5a5qj': ['stm32u5a5__', 4096, 2514],
    'stm32u5a5rj': ['stm32u5a5__', 4096, 2514],
    'stm32u5a5vj': ['stm32u5a5__', 4096, 2514],
    'stm32u5a5zj': ['stm32u5a5__', 4096, 2514],
    'stm32u5a9bj': ['stm32u5a9__', 4096, 2514],
    'stm32u5a9nj': ['stm32u5a9__', 4096, 2514],
    'stm32u5a9vj': ['stm32u5a9__', 4096, 2514],
    'stm32u5a9zj': ['stm32u5a9__', 4096, 2514],
    'stm32u5f7vj': ['stm32u5f7__', 4096, 3024],
    'stm32u5f9bj': ['stm32u5f9__', 4096, 3024],
    'stm32u5f9nj': ['stm32u5f9__', 4096, 3024],
    'stm32u5f9vj': ['stm32u5f9__', 4096, 3024],
    'stm32u5f9zj': ['stm32u5f9__', 4096, 3024],
    'stm32u5g7vj': ['stm32u5g7__', 4096, 3024],
    'stm32u5g9bj': ['stm32u5g9__', 4096, 3024],
    'stm32u5g9nj': ['stm32u5g9__', 4096, 3024],
    'stm32u5g9vj': ['stm32u5g9__', 4096, 3024],
    'stm32u5g9zj': ['stm32u5g9__', 4096, 3024],
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