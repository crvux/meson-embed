project('st-cmsis-g4', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.2.5',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-g4-1.2.5'

mcus = {
    'stm32g431c6': 'stm32g431__',
    'stm32g431c8': 'stm32g431__',
    'stm32g431cb': 'stm32g431__',
    'stm32g431k6': 'stm32g431__',
    'stm32g431k8': 'stm32g431__',
    'stm32g431kb': 'stm32g431__',
    'stm32g431m6': 'stm32g431__',
    'stm32g431m8': 'stm32g431__',
    'stm32g431mb': 'stm32g431__',
    'stm32g431r6': 'stm32g431__',
    'stm32g431r8': 'stm32g431__',
    'stm32g431rb': 'stm32g431__',
    'stm32g431v6': 'stm32g431__',
    'stm32g431v8': 'stm32g431__',
    'stm32g431vb': 'stm32g431__',
    'stm32g441cb': 'stm32g441__',
    'stm32g441kb': 'stm32g441__',
    'stm32g441mb': 'stm32g441__',
    'stm32g441rb': 'stm32g441__',
    'stm32g441vb': 'stm32g441__',
    'stm32g473cb': 'stm32g473__',
    'stm32g473cc': 'stm32g473__',
    'stm32g473ce': 'stm32g473__',
    'stm32g473mb': 'stm32g473__',
    'stm32g473mc': 'stm32g473__',
    'stm32g473me': 'stm32g473__',
    'stm32g473pb': 'stm32g473__',
    'stm32g473pc': 'stm32g473__',
    'stm32g473pe': 'stm32g473__',
    'stm32g473qb': 'stm32g473__',
    'stm32g473qc': 'stm32g473__',
    'stm32g473qe': 'stm32g473__',
    'stm32g473rb': 'stm32g473__',
    'stm32g473rc': 'stm32g473__',
    'stm32g473re': 'stm32g473__',
    'stm32g473vb': 'stm32g473__',
    'stm32g473vc': 'stm32g473__',
    'stm32g473ve': 'stm32g473__',
    'stm32g474cb': 'stm32g474__',
    'stm32g474cc': 'stm32g474__',
    'stm32g474ce': 'stm32g474__',
    'stm32g474mb': 'stm32g474__',
    'stm32g474mc': 'stm32g474__',
    'stm32g474me': 'stm32g474__',
    'stm32g474pb': 'stm32g474__',
    'stm32g474pc': 'stm32g474__',
    'stm32g474pe': 'stm32g474__',
    'stm32g474qb': 'stm32g474__',
    'stm32g474qc': 'stm32g474__',
    'stm32g474qe': 'stm32g474__',
    'stm32g474rb': 'stm32g474__',
    'stm32g474rc': 'stm32g474__',
    'stm32g474re': 'stm32g474__',
    'stm32g474vb': 'stm32g474__',
    'stm32g474vc': 'stm32g474__',
    'stm32g474ve': 'stm32g474__',
    'stm32g483ce': 'stm32g483__',
    'stm32g483me': 'stm32g483__',
    'stm32g483pe': 'stm32g483__',
    'stm32g483qe': 'stm32g483__',
    'stm32g483re': 'stm32g483__',
    'stm32g483ve': 'stm32g483__',
    'stm32g484ce': 'stm32g484__',
    'stm32g484me': 'stm32g484__',
    'stm32g484pe': 'stm32g484__',
    'stm32g484qe': 'stm32g484__',
    'stm32g484re': 'stm32g484__',
    'stm32g484ve': 'stm32g484__',
    'stm32g491cc': 'stm32g491__',
    'stm32g491ce': 'stm32g491__',
    'stm32g491kc': 'stm32g491__',
    'stm32g491ke': 'stm32g491__',
    'stm32g491mc': 'stm32g491__',
    'stm32g491me': 'stm32g491__',
    'stm32g491rc': 'stm32g491__',
    'stm32g491re': 'stm32g491__',
    'stm32g491vc': 'stm32g491__',
    'stm32g491ve': 'stm32g491__',
    'stm32g4a1ce': 'stm32g4a1__',
    'stm32g4a1ke': 'stm32g4a1__',
    'stm32g4a1me': 'stm32g4a1__',
    'stm32g4a1re': 'stm32g4a1__',
    'stm32g4a1ve': 'stm32g4a1__',
}
mcu = meson.get_external_property('mcu')
if mcu not in mcus
  mcus_keys = mcus.keys()
  error(f'Please specify external property \'mcu\' with one of this values: @mcus_keys@')
endif
type = mcus[mcu]

type_lower = type.replace('_', 'x')
system_src = files(root / 'Source' / 'Templates' / 'system_stm32g4xx.c')
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

st_cmsis_g4_type_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  compile_args: [type_define, pkg_define],
  link_with: lib,
)
st_cmsis_g4_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: st_cmsis_g4_type_dep,
  link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
)