project('st-cmsis-l1', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '2.3.4',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-l1-2.3.4'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32l100_b', 
  'stm32l100_ba', 
  'stm32l100_c', 
  'stm32l151_b', 
  'stm32l151_ba', 
  'stm32l151_c', 
  'stm32l151_ca', 
  'stm32l151_d', 
  'stm32l151_dx', 
  'stm32l151_e', 
  'stm32l152_b', 
  'stm32l152_ba', 
  'stm32l152_c', 
  'stm32l152_ca', 
  'stm32l152_d', 
  'stm32l152_dx', 
  'stm32l152_e', 
  'stm32l162_c', 
  'stm32l162_ca', 
  'stm32l162_d', 
  'stm32l162_dx', 
  'stm32l162_e', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32l1xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32L1'  # This can be used with universal stm32 header 

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
    'stm32l100c6a': ['stm32l100_ba', 32, 4],
    'stm32l100r8a': ['stm32l100_ba', 64, 8],
    'stm32l100rba': ['stm32l100_ba', 128, 16],
    'stm32l100rc': ['stm32l100_c', 256, 16],
    'stm32l151c6a': ['stm32l151_ba', 32, 16],
    'stm32l151c8a': ['stm32l151_ba', 64, 32],
    'stm32l151cba': ['stm32l151_ba', 128, 32],
    'stm32l151cc': ['stm32l151_c', 256, 32],
    'stm32l151qc': ['stm32l151_c', 256, 32],
    'stm32l151qd': ['stm32l151_d', 384, 48],
    'stm32l151qe': ['stm32l151_e', 512, 80],
    'stm32l151r6a': ['stm32l151_ba', 32, 16],
    'stm32l151r8a': ['stm32l151_ba', 64, 32],
    'stm32l151rba': ['stm32l151_ba', 128, 32],
    'stm32l151rc': ['stm32l151_c', 256, 32],
    'stm32l151rd': ['stm32l151_d', 384, 48],
    'stm32l151re': ['stm32l151_e', 512, 80],
    'stm32l151uc': ['stm32l151_c', 256, 32],
    'stm32l151v8a': ['stm32l151_ba', 64, 32],
    'stm32l151vba': ['stm32l151_ba', 128, 32],
    'stm32l151vc': ['stm32l151_c', 256, 32],
    'stm32l151vd': ['stm32l151_d', 384, 48],
    'stm32l151vdx': ['stm32l151_dx', 384, 80],
    'stm32l151ve': ['stm32l151_e', 512, 80],
    'stm32l151zc': ['stm32l151_c', 256, 32],
    'stm32l151zd': ['stm32l151_d', 384, 48],
    'stm32l151ze': ['stm32l151_e', 512, 80],
    'stm32l152c6a': ['stm32l152_ba', 32, 16],
    'stm32l152c8a': ['stm32l152_ba', 64, 32],
    'stm32l152cba': ['stm32l152_ba', 128, 32],
    'stm32l152cc': ['stm32l152_c', 256, 32],
    'stm32l152qc': ['stm32l152_c', 256, 32],
    'stm32l152qd': ['stm32l152_d', 384, 48],
    'stm32l152qe': ['stm32l152_e', 512, 80],
    'stm32l152r6a': ['stm32l152_ba', 32, 16],
    'stm32l152r8a': ['stm32l152_ba', 64, 32],
    'stm32l152rba': ['stm32l152_ba', 128, 32],
    'stm32l152rc': ['stm32l152_c', 256, 32],
    'stm32l152rd': ['stm32l152_d', 384, 48],
    'stm32l152re': ['stm32l152_e', 512, 80],
    'stm32l152uc': ['stm32l152_c', 256, 32],
    'stm32l152v8a': ['stm32l152_ba', 64, 32],
    'stm32l152vba': ['stm32l152_ba', 128, 32],
    'stm32l152vc': ['stm32l152_c', 256, 32],
    'stm32l152vd': ['stm32l152_d', 384, 48],
    'stm32l152vdx': ['stm32l152_dx', 384, 80],
    'stm32l152ve': ['stm32l152_e', 512, 80],
    'stm32l152zc': ['stm32l152_c', 256, 32],
    'stm32l152zd': ['stm32l152_d', 384, 48],
    'stm32l152ze': ['stm32l152_e', 512, 80],
    'stm32l162qc': ['stm32l162_c', 256, 32],
    'stm32l162qd': ['stm32l162_d', 384, 48],
    'stm32l162rc': ['stm32l162_c', 256, 32],
    'stm32l162rd': ['stm32l162_d', 384, 48],
    'stm32l162re': ['stm32l162_e', 512, 80],
    'stm32l162vc': ['stm32l162_c', 256, 32],
    'stm32l162vd': ['stm32l162_d', 384, 48],
    'stm32l162vdx': ['stm32l162_dx', 384, 80],
    'stm32l162ve': ['stm32l162_e', 512, 80],
    'stm32l162zc': ['stm32l162_c', 256, 32],
    'stm32l162zd': ['stm32l162_d', 384, 48],
    'stm32l162ze': ['stm32l162_e', 512, 80],
    'stm32l100c6': ['stm32l100_b', 32, 4],
    'stm32l100r8': ['stm32l100_b', 64, 8],
    'stm32l100rb': ['stm32l100_b', 128, 10],
    'stm32l151c6': ['stm32l151_b', 32, 10],
    'stm32l151c8': ['stm32l151_b', 64, 10],
    'stm32l151cb': ['stm32l151_b', 128, 16],
    'stm32l151r6': ['stm32l151_b', 32, 10],
    'stm32l151r8': ['stm32l151_b', 64, 10],
    'stm32l151rb': ['stm32l151_b', 128, 16],
    'stm32l151rca': ['stm32l151_ca', 256, 32],
    'stm32l151v8': ['stm32l151_b', 64, 10],
    'stm32l151vb': ['stm32l151_b', 128, 16],
    'stm32l151vca': ['stm32l151_ca', 256, 32],
    'stm32l152c6': ['stm32l152_b', 32, 10],
    'stm32l152c8': ['stm32l152_b', 64, 10],
    'stm32l152cb': ['stm32l152_b', 128, 16],
    'stm32l152r6': ['stm32l152_b', 32, 10],
    'stm32l152r8': ['stm32l152_b', 64, 10],
    'stm32l152rb': ['stm32l152_b', 128, 16],
    'stm32l152rca': ['stm32l152_ca', 256, 32],
    'stm32l152v8': ['stm32l152_b', 64, 10],
    'stm32l152vb': ['stm32l152_b', 128, 16],
    'stm32l152vca': ['stm32l152_ca', 256, 32],
    'stm32l162rca': ['stm32l162_ca', 256, 32],
    'stm32l162vca': ['stm32l162_ca', 256, 32],
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