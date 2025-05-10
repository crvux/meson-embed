project('st-cmsis-f3', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '2.3.8',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-f3-2.3.8'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32f301_8', 
  'stm32f302_8', 
  'stm32f302_c', 
  'stm32f302_e', 
  'stm32f303_8', 
  'stm32f303_c', 
  'stm32f303_e', 
  'stm32f373_c', 
  'stm32f334_8', 
  'stm32f318__', 
  'stm32f328__', 
  'stm32f358__', 
  'stm32f378__', 
  'stm32f398__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32f3xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32F3'  # This can be used with universal stm32 header 

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
    'stm32f301c6': ['stm32f301_8', 32, 16],
    'stm32f301c8': ['stm32f301_8', 64, 16],
    'stm32f301k6': ['stm32f301_8', 32, 16],
    'stm32f301k8': ['stm32f301_8', 64, 16],
    'stm32f301r6': ['stm32f301_8', 32, 16],
    'stm32f301r8': ['stm32f301_8', 64, 16],
    'stm32f302c6': ['stm32f302_8', 32, 16],
    'stm32f302c8': ['stm32f302_8', 64, 16],
    'stm32f302cb': ['stm32f302_c', 128, 32],
    'stm32f302cc': ['stm32f302_c', 256, 40],
    'stm32f302k6': ['stm32f302_8', 32, 16],
    'stm32f302k8': ['stm32f302_8', 64, 16],
    'stm32f302r6': ['stm32f302_8', 32, 16],
    'stm32f302r8': ['stm32f302_8', 64, 16],
    'stm32f302rb': ['stm32f302_c', 128, 32],
    'stm32f302rc': ['stm32f302_c', 256, 40],
    'stm32f302rd': ['stm32f302_e', 384, 64],
    'stm32f302re': ['stm32f302_e', 512, 64],
    'stm32f302vb': ['stm32f302_c', 128, 32],
    'stm32f302vc': ['stm32f302_c', 256, 40],
    'stm32f302vd': ['stm32f302_e', 384, 64],
    'stm32f302ve': ['stm32f302_e', 512, 64],
    'stm32f302zd': ['stm32f302_e', 384, 64],
    'stm32f302ze': ['stm32f302_e', 512, 64],
    'stm32f303c6': ['stm32f303_8', 32, 16],
    'stm32f303c8': ['stm32f303_8', 64, 16],
    'stm32f303cb': ['stm32f303_c', 128, 40],
    'stm32f303cc': ['stm32f303_c', 256, 48],
    'stm32f303k6': ['stm32f303_8', 32, 16],
    'stm32f303k8': ['stm32f303_8', 64, 16],
    'stm32f303r6': ['stm32f303_8', 32, 16],
    'stm32f303r8': ['stm32f303_8', 64, 16],
    'stm32f303rb': ['stm32f303_c', 128, 40],
    'stm32f303rc': ['stm32f303_c', 256, 48],
    'stm32f303rd': ['stm32f303_e', 384, 80],
    'stm32f303re': ['stm32f303_e', 512, 80],
    'stm32f303vb': ['stm32f303_c', 128, 40],
    'stm32f303vc': ['stm32f303_c', 256, 48],
    'stm32f303vd': ['stm32f303_e', 384, 80],
    'stm32f303ve': ['stm32f303_e', 512, 80],
    'stm32f303zd': ['stm32f303_e', 384, 80],
    'stm32f303ze': ['stm32f303_e', 512, 80],
    'stm32f318c8': ['stm32f318__', 64, 16],
    'stm32f318k8': ['stm32f318__', 64, 16],
    'stm32f328c8': ['stm32f328__', 64, 16],
    'stm32f334c4': ['stm32f334_8', 16, 16],
    'stm32f334c6': ['stm32f334_8', 32, 16],
    'stm32f334c8': ['stm32f334_8', 64, 16],
    'stm32f334k4': ['stm32f334_8', 16, 16],
    'stm32f334k6': ['stm32f334_8', 32, 16],
    'stm32f334k8': ['stm32f334_8', 64, 16],
    'stm32f334r6': ['stm32f334_8', 32, 16],
    'stm32f334r8': ['stm32f334_8', 64, 16],
    'stm32f358cc': ['stm32f358__', 256, 48],
    'stm32f358rc': ['stm32f358__', 256, 48],
    'stm32f358vc': ['stm32f358__', 256, 48],
    'stm32f373c8': ['stm32f373_c', 64, 16],
    'stm32f373cb': ['stm32f373_c', 128, 24],
    'stm32f373cc': ['stm32f373_c', 256, 32],
    'stm32f373r8': ['stm32f373_c', 64, 16],
    'stm32f373rb': ['stm32f373_c', 128, 24],
    'stm32f373rc': ['stm32f373_c', 256, 32],
    'stm32f373v8': ['stm32f373_c', 64, 16],
    'stm32f373vb': ['stm32f373_c', 128, 24],
    'stm32f373vc': ['stm32f373_c', 256, 32],
    'stm32f378cc': ['stm32f378__', 256, 32],
    'stm32f378rc': ['stm32f378__', 256, 32],
    'stm32f378vc': ['stm32f378__', 256, 32],
    'stm32f398ve': ['stm32f398__', 512, 80],
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