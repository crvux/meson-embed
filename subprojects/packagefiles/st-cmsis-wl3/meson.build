project('st-cmsis-wl3', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.1.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-wl3-1.1.0'

mcus = {
    'stm32wl33c8': 'stm32wl3__',
    'stm32wl33cb': 'stm32wl3__',
    'stm32wl33cc': 'stm32wl3__',
    'stm32wl33k8': 'stm32wl3__',
    'stm32wl33kb': 'stm32wl3__',
    'stm32wl33kc': 'stm32wl3__',
}
mcu = meson.get_external_property('mcu')
if mcu not in mcus
  mcus_keys = mcus.keys()
  error(f'Please specify external property \'mcu\' with one of this values: @mcus_keys@')
endif
type = mcus[mcu]

type_lower = type.replace('_', 'x')
system_src = files(root / 'Source' / 'Templates' / 'system_stm32wl3xx.c')
startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
type_define = '-D' + type.to_upper().replace('_', 'x')
pkg_define = '-DSTM32WL3'  # This can be used with universal stm32 header 

lib = static_library(
  f'st_cmsis_@type_lower@', system_src, startup_src,
  build_by_default: false,
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  c_args: type_define,
  cpp_args: type_define,
)

st_cmsis_wl3_type_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  compile_args: [type_define, pkg_define],
  link_with: lib,
)
st_cmsis_wl3_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: st_cmsis_wl3_type_dep,
  link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
)