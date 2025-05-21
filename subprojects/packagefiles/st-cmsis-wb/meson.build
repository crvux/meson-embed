project('st-cmsis-wb', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.12.2',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-wb-1.12.2'

mcus = {
    'stm32wb10cc': 'stm32wb10__',
    'stm32wb15cc': 'stm32wb15__',
    'stm32wb1mmc': 'stm32wb1m__',
    'stm32wb30ce': 'stm32wb30__',
    'stm32wb35cc': 'stm32wb35__',
    'stm32wb35ce': 'stm32wb35__',
    'stm32wb50cg': 'stm32wb50__',
    'stm32wb55cc': 'stm32wb55__',
    'stm32wb55ce': 'stm32wb55__',
    'stm32wb55cg': 'stm32wb55__',
    'stm32wb55rc': 'stm32wb55__',
    'stm32wb55re': 'stm32wb55__',
    'stm32wb55rg': 'stm32wb55__',
    'stm32wb55vc': 'stm32wb55__',
    'stm32wb55ve': 'stm32wb55__',
    'stm32wb55vg': 'stm32wb55__',
    'stm32wb55vy': 'stm32wb55__',
    'stm32wb5mmg': 'stm32wb5m__',
}
mcu = meson.get_external_property('mcu')
if mcu not in mcus
  mcus_keys = mcus.keys()
  error(f'Please specify external property \'mcu\' with one of this values: @mcus_keys@')
endif
type = mcus[mcu]

type_lower = type.replace('_', 'x')
system_src = files(root / 'Source' / 'Templates' / 'system_stm32wbxx.c')
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

st_cmsis_wb_type_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  compile_args: [type_define, pkg_define],
  link_with: lib,
)
st_cmsis_wb_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: st_cmsis_wb_type_dep,
  link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
)