project('st-cmsis-u3', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.1.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-u3-1.1.0'

mcus = {
    'stm32u375ce': 'stm32u375__',
    'stm32u375cg': 'stm32u375__',
    'stm32u375ke': 'stm32u375__',
    'stm32u375kg': 'stm32u375__',
    'stm32u375re': 'stm32u375__',
    'stm32u375rg': 'stm32u375__',
    'stm32u375ve': 'stm32u375__',
    'stm32u375vg': 'stm32u375__',
    'stm32u385cg': 'stm32u385__',
    'stm32u385kg': 'stm32u385__',
    'stm32u385rg': 'stm32u385__',
    'stm32u385vg': 'stm32u385__',
}
mcu = meson.get_external_property('mcu')
if mcu not in mcus
  mcus_keys = mcus.keys()
  error(f'Please specify external property \'mcu\' with one of this values: @mcus_keys@')
endif
type = mcus[mcu]

type_lower = type.replace('_', 'x')
system_src = files(root / 'Source' / 'Templates' / 'system_stm32u3xx.c')
startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
type_define = '-D' + type.to_upper().replace('_', 'x')
pkg_define = '-DSTM32U3'  # This can be used with universal stm32 header 

lib = static_library(
  f'st_cmsis_@type_lower@', system_src, startup_src,
  build_by_default: false,
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  c_args: type_define,
  cpp_args: type_define,
)

st_cmsis_u3_type_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  compile_args: [type_define, pkg_define],
  link_with: lib,
)
st_cmsis_u3_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: st_cmsis_u3_type_dep,
  link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
)