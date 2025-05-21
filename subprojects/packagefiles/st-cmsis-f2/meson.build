project('st-cmsis-f2', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '2.2.6',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-f2-2.2.6'

mcus = {
    'stm32f205rb': 'stm32f205__',
    'stm32f205rc': 'stm32f205__',
    'stm32f205re': 'stm32f205__',
    'stm32f205rf': 'stm32f205__',
    'stm32f205rg': 'stm32f205__',
    'stm32f205vb': 'stm32f205__',
    'stm32f205vc': 'stm32f205__',
    'stm32f205ve': 'stm32f205__',
    'stm32f205vf': 'stm32f205__',
    'stm32f205vg': 'stm32f205__',
    'stm32f205zc': 'stm32f205__',
    'stm32f205ze': 'stm32f205__',
    'stm32f205zf': 'stm32f205__',
    'stm32f205zg': 'stm32f205__',
    'stm32f207ic': 'stm32f207__',
    'stm32f207ie': 'stm32f207__',
    'stm32f207if': 'stm32f207__',
    'stm32f207ig': 'stm32f207__',
    'stm32f207vc': 'stm32f207__',
    'stm32f207ve': 'stm32f207__',
    'stm32f207vf': 'stm32f207__',
    'stm32f207vg': 'stm32f207__',
    'stm32f207zc': 'stm32f207__',
    'stm32f207ze': 'stm32f207__',
    'stm32f207zf': 'stm32f207__',
    'stm32f207zg': 'stm32f207__',
    'stm32f215re': 'stm32f215__',
    'stm32f215rg': 'stm32f215__',
    'stm32f215ve': 'stm32f215__',
    'stm32f215vg': 'stm32f215__',
    'stm32f215ze': 'stm32f215__',
    'stm32f215zg': 'stm32f215__',
    'stm32f217ie': 'stm32f217__',
    'stm32f217ig': 'stm32f217__',
    'stm32f217ve': 'stm32f217__',
    'stm32f217vg': 'stm32f217__',
    'stm32f217ze': 'stm32f217__',
    'stm32f217zg': 'stm32f217__',
}
mcu = meson.get_external_property('mcu')
if mcu not in mcus
  mcus_keys = mcus.keys()
  error(f'Please specify external property \'mcu\' with one of this values: @mcus_keys@')
endif
type = mcus[mcu]

type_lower = type.replace('_', 'x')
system_src = files(root / 'Source' / 'Templates' / 'system_stm32f2xx.c')
startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
type_define = '-D' + type.to_upper().replace('_', 'x')
pkg_define = '-DSTM32F2'  # This can be used with universal stm32 header 

lib = static_library(
  f'st_cmsis_@type_lower@', system_src, startup_src,
  build_by_default: false,
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  c_args: type_define,
  cpp_args: type_define,
)

st_cmsis_f2_type_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: [st_cmsis_core],
  include_directories: root / 'Include',
  compile_args: [type_define, pkg_define],
  link_with: lib,
)
st_cmsis_f2_dep = declare_dependency(
  version: meson.project_version(),
  dependencies: st_cmsis_f2_type_dep,
  link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
)