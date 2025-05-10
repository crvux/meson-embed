project('st-cmsis-g4', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.2.5',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-g4-1.2.5'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32g431__', 
  'stm32g441__', 
  'stm32g471__', 
  'stm32g473__', 
  'stm32g483__', 
  'stm32g474__', 
  'stm32g484__', 
  'stm32g491__', 
  'stm32g4a1__', 
  'stm32gbk1cb', 
  'stm32g411_b', 
  'stm32g411_c', 
  'stm32g414__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32g4xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32G4'  # This can be used with universal stm32 header 

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
    'stm32g431c6': ['stm32g431__', 32, 32],
    'stm32g431c8': ['stm32g431__', 64, 32],
    'stm32g431cb': ['stm32g431__', 128, 32],
    'stm32g431k6': ['stm32g431__', 32, 32],
    'stm32g431k8': ['stm32g431__', 64, 32],
    'stm32g431kb': ['stm32g431__', 128, 32],
    'stm32g431m6': ['stm32g431__', 32, 32],
    'stm32g431m8': ['stm32g431__', 64, 32],
    'stm32g431mb': ['stm32g431__', 128, 32],
    'stm32g431r6': ['stm32g431__', 32, 32],
    'stm32g431r8': ['stm32g431__', 64, 32],
    'stm32g431rb': ['stm32g431__', 128, 32],
    'stm32g431v6': ['stm32g431__', 32, 32],
    'stm32g431v8': ['stm32g431__', 64, 32],
    'stm32g431vb': ['stm32g431__', 128, 32],
    'stm32g441cb': ['stm32g441__', 128, 32],
    'stm32g441kb': ['stm32g441__', 128, 32],
    'stm32g441mb': ['stm32g441__', 128, 32],
    'stm32g441rb': ['stm32g441__', 128, 32],
    'stm32g441vb': ['stm32g441__', 128, 32],
    'stm32g473cb': ['stm32g473__', 128, 128],
    'stm32g473cc': ['stm32g473__', 256, 128],
    'stm32g473ce': ['stm32g473__', 512, 128],
    'stm32g473mb': ['stm32g473__', 128, 128],
    'stm32g473mc': ['stm32g473__', 256, 128],
    'stm32g473me': ['stm32g473__', 512, 128],
    'stm32g473pb': ['stm32g473__', 128, 128],
    'stm32g473pc': ['stm32g473__', 256, 128],
    'stm32g473pe': ['stm32g473__', 512, 128],
    'stm32g473qb': ['stm32g473__', 128, 128],
    'stm32g473qc': ['stm32g473__', 256, 128],
    'stm32g473qe': ['stm32g473__', 512, 128],
    'stm32g473rb': ['stm32g473__', 128, 128],
    'stm32g473rc': ['stm32g473__', 256, 128],
    'stm32g473re': ['stm32g473__', 512, 128],
    'stm32g473vb': ['stm32g473__', 128, 128],
    'stm32g473vc': ['stm32g473__', 256, 128],
    'stm32g473ve': ['stm32g473__', 512, 128],
    'stm32g474cb': ['stm32g474__', 128, 128],
    'stm32g474cc': ['stm32g474__', 256, 128],
    'stm32g474ce': ['stm32g474__', 512, 128],
    'stm32g474mb': ['stm32g474__', 128, 128],
    'stm32g474mc': ['stm32g474__', 256, 128],
    'stm32g474me': ['stm32g474__', 512, 128],
    'stm32g474pb': ['stm32g474__', 128, 128],
    'stm32g474pc': ['stm32g474__', 256, 128],
    'stm32g474pe': ['stm32g474__', 128, 128],
    'stm32g474qb': ['stm32g474__', 128, 128],
    'stm32g474qc': ['stm32g474__', 256, 128],
    'stm32g474qe': ['stm32g474__', 512, 128],
    'stm32g474rb': ['stm32g474__', 128, 128],
    'stm32g474rc': ['stm32g474__', 256, 128],
    'stm32g474re': ['stm32g474__', 512, 128],
    'stm32g474vb': ['stm32g474__', 128, 128],
    'stm32g474vc': ['stm32g474__', 256, 128],
    'stm32g474ve': ['stm32g474__', 512, 128],
    'stm32g483ce': ['stm32g483__', 512, 128],
    'stm32g483me': ['stm32g483__', 512, 128],
    'stm32g483pe': ['stm32g483__', 512, 128],
    'stm32g483qe': ['stm32g483__', 512, 128],
    'stm32g483re': ['stm32g483__', 512, 128],
    'stm32g483ve': ['stm32g483__', 512, 128],
    'stm32g484ce': ['stm32g484__', 512, 128],
    'stm32g484me': ['stm32g484__', 512, 128],
    'stm32g484pe': ['stm32g484__', 512, 128],
    'stm32g484qe': ['stm32g484__', 512, 128],
    'stm32g484re': ['stm32g484__', 512, 128],
    'stm32g484ve': ['stm32g484__', 512, 128],
    'stm32g491cc': ['stm32g491__', 256, 112],
    'stm32g491ce': ['stm32g491__', 512, 112],
    'stm32g491kc': ['stm32g491__', 256, 112],
    'stm32g491ke': ['stm32g491__', 512, 112],
    'stm32g491mc': ['stm32g491__', 256, 112],
    'stm32g491me': ['stm32g491__', 512, 112],
    'stm32g491rc': ['stm32g491__', 256, 112],
    'stm32g491re': ['stm32g491__', 512, 112],
    'stm32g491vc': ['stm32g491__', 256, 112],
    'stm32g491ve': ['stm32g491__', 512, 112],
    'stm32g4a1ce': ['stm32g4a1__', 512, 112],
    'stm32g4a1ke': ['stm32g4a1__', 512, 112],
    'stm32g4a1me': ['stm32g4a1__', 512, 112],
    'stm32g4a1re': ['stm32g4a1__', 512, 112],
    'stm32g4a1ve': ['stm32g4a1__', 512, 112],
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