project('st-cmsis-l0', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.9.4',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-l0-1.9.4'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32l010_b', 
  'stm32l010_8', 
  'stm32l010_6', 
  'stm32l010_4', 
  'stm32l011__', 
  'stm32l021__', 
  'stm32l031__', 
  'stm32l041__', 
  'stm32l051__', 
  'stm32l052__', 
  'stm32l053__', 
  'stm32l062__', 
  'stm32l063__', 
  'stm32l071__', 
  'stm32l072__', 
  'stm32l073__', 
  'stm32l082__', 
  'stm32l083__', 
  'stm32l081__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32l0xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32L0'  # This can be used with universal stm32 header 

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
    'stm32l010c6': ['stm32l010_6', 32, 8],
    'stm32l010f4': ['stm32l010_4', 16, 2],
    'stm32l010k4': ['stm32l010_4', 16, 2],
    'stm32l010k8': ['stm32l010_8', 64, 8],
    'stm32l010r8': ['stm32l010_8', 64, 8],
    'stm32l010rb': ['stm32l010_b', 128, 20],
    'stm32l011d3': ['stm32l011__', 8, 2],
    'stm32l011d4': ['stm32l011__', 16, 2],
    'stm32l011e3': ['stm32l011__', 8, 2],
    'stm32l011e4': ['stm32l011__', 16, 2],
    'stm32l011f3': ['stm32l011__', 8, 2],
    'stm32l011f4': ['stm32l011__', 16, 2],
    'stm32l011g3': ['stm32l011__', 8, 2],
    'stm32l011g4': ['stm32l011__', 16, 2],
    'stm32l011k3': ['stm32l011__', 8, 2],
    'stm32l011k4': ['stm32l011__', 16, 2],
    'stm32l021d4': ['stm32l021__', 16, 2],
    'stm32l021f4': ['stm32l021__', 16, 2],
    'stm32l021g4': ['stm32l021__', 16, 2],
    'stm32l021k4': ['stm32l021__', 16, 2],
    'stm32l031c4': ['stm32l031__', 16, 8],
    'stm32l031c6': ['stm32l031__', 32, 8],
    'stm32l031e4': ['stm32l031__', 16, 8],
    'stm32l031e6': ['stm32l031__', 32, 8],
    'stm32l031f4': ['stm32l031__', 16, 8],
    'stm32l031f6': ['stm32l031__', 32, 8],
    'stm32l031g4': ['stm32l031__', 16, 8],
    'stm32l031g6': ['stm32l031__', 32, 8],
    'stm32l031k4': ['stm32l031__', 16, 8],
    'stm32l031k6': ['stm32l031__', 32, 8],
    'stm32l041c6': ['stm32l041__', 32, 8],
    'stm32l041e6': ['stm32l041__', 32, 8],
    'stm32l041f6': ['stm32l041__', 32, 8],
    'stm32l041g6': ['stm32l041__', 32, 8],
    'stm32l041k6': ['stm32l041__', 32, 8],
    'stm32l051c6': ['stm32l051__', 32, 8],
    'stm32l051c8': ['stm32l051__', 64, 8],
    'stm32l051k6': ['stm32l051__', 32, 8],
    'stm32l051k8': ['stm32l051__', 64, 8],
    'stm32l051r6': ['stm32l051__', 32, 8],
    'stm32l051r8': ['stm32l051__', 64, 8],
    'stm32l051t6': ['stm32l051__', 32, 8],
    'stm32l051t8': ['stm32l051__', 64, 8],
    'stm32l052c6': ['stm32l052__', 32, 8],
    'stm32l052c8': ['stm32l052__', 64, 8],
    'stm32l052k6': ['stm32l052__', 32, 8],
    'stm32l052k8': ['stm32l052__', 64, 8],
    'stm32l052r6': ['stm32l052__', 32, 8],
    'stm32l052r8': ['stm32l052__', 64, 8],
    'stm32l052t6': ['stm32l052__', 32, 8],
    'stm32l052t8': ['stm32l052__', 64, 8],
    'stm32l053c6': ['stm32l053__', 32, 8],
    'stm32l053c8': ['stm32l053__', 64, 8],
    'stm32l053r6': ['stm32l053__', 32, 8],
    'stm32l053r8': ['stm32l053__', 64, 8],
    'stm32l062c8': ['stm32l062__', 64, 8],
    'stm32l062k8': ['stm32l062__', 64, 8],
    'stm32l063c8': ['stm32l063__', 64, 8],
    'stm32l063r8': ['stm32l063__', 64, 8],
    'stm32l071c8': ['stm32l071__', 64, 20],
    'stm32l071cb': ['stm32l071__', 128, 20],
    'stm32l071cz': ['stm32l071__', 192, 20],
    'stm32l071k8': ['stm32l071__', 64, 20],
    'stm32l071kb': ['stm32l071__', 128, 20],
    'stm32l071kz': ['stm32l071__', 192, 20],
    'stm32l071rb': ['stm32l071__', 128, 20],
    'stm32l071rz': ['stm32l071__', 192, 20],
    'stm32l071v8': ['stm32l071__', 64, 20],
    'stm32l071vb': ['stm32l071__', 128, 20],
    'stm32l071vz': ['stm32l071__', 192, 20],
    'stm32l072cb': ['stm32l072__', 128, 20],
    'stm32l072cz': ['stm32l072__', 192, 20],
    'stm32l072kb': ['stm32l072__', 128, 20],
    'stm32l072kz': ['stm32l072__', 192, 20],
    'stm32l072rb': ['stm32l072__', 128, 20],
    'stm32l072rz': ['stm32l072__', 192, 20],
    'stm32l072v8': ['stm32l072__', 64, 20],
    'stm32l072vb': ['stm32l072__', 128, 20],
    'stm32l072vz': ['stm32l072__', 192, 20],
    'stm32l073cb': ['stm32l073__', 128, 20],
    'stm32l073cz': ['stm32l073__', 192, 20],
    'stm32l073rb': ['stm32l073__', 128, 20],
    'stm32l073rz': ['stm32l073__', 192, 20],
    'stm32l073v8': ['stm32l073__', 64, 20],
    'stm32l073vb': ['stm32l073__', 128, 20],
    'stm32l073vz': ['stm32l073__', 192, 20],
    'stm32l081cb': ['stm32l081__', 128, 20],
    'stm32l081cz': ['stm32l081__', 192, 20],
    'stm32l081kz': ['stm32l081__', 192, 20],
    'stm32l082cz': ['stm32l082__', 192, 20],
    'stm32l082kb': ['stm32l082__', 128, 20],
    'stm32l082kz': ['stm32l082__', 192, 20],
    'stm32l083cb': ['stm32l083__', 128, 20],
    'stm32l083cz': ['stm32l083__', 192, 20],
    'stm32l083rb': ['stm32l083__', 128, 20],
    'stm32l083rz': ['stm32l083__', 192, 20],
    'stm32l083v8': ['stm32l083__', 64, 20],
    'stm32l083vb': ['stm32l083__', 128, 20],
    'stm32l083vz': ['stm32l083__', 192, 20],
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