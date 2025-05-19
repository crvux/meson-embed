project('st-cmsis-h5', 'c', 'cpp',
  license: 'Apache-2.0',
  meson_version: '>=1.5.1',
  version: '1.4.0',
  default_options: ['default_library=static', 'b_staticpic=false'],
)

st_cmsis_core = dependency('st-cmsis-core', version: '5.9.0')
root = 'cmsis-device-h5-1.4.0'

# Per type dependencies with -D compiler flags and startup sources
types = [
  'stm32h503__', 
  'stm32h523__', 
  'stm32h533__', 
  'stm32h562__', 
  'stm32h563__', 
  'stm32h573__', 
]
system_src = files(root / 'Source' / 'Templates' / 'system_stm32h5xx.c')
foreach type : types
  type_lower = type.replace('_', 'x')
  startup_src = files(root / 'Source' / 'Templates' / 'gcc' / f'startup_@type_lower@.s')
  type_define = '-D' + type.to_upper().replace('_', 'x')
  pkg_define = '-DSTM32H5'  # This can be used with universal stm32 header 

  lib = static_library(
    f'st_cmsis_@type_lower@', system_src, startup_src,
    build_by_default: false,
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    c_args: type_define,
    cpp_args: type_define,
  )
  set_variable(f'st_cmsis_@type@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: [st_cmsis_core],
    include_directories: root / 'Include',
    compile_args: [type_define, pkg_define],
    link_with: lib,
  ))
endforeach

# Per MCU dependencies with default linker script
mcus = {
    'stm32h503cb': 'stm32h503__',
    'stm32h503eb': 'stm32h503__',
    'stm32h503kb': 'stm32h503__',
    'stm32h503rb': 'stm32h503__',
    'stm32h523cc': 'stm32h523__',
    'stm32h523ce': 'stm32h523__',
    'stm32h523he': 'stm32h523__',
    'stm32h523rc': 'stm32h523__',
    'stm32h523re': 'stm32h523__',
    'stm32h523vc': 'stm32h523__',
    'stm32h523ve': 'stm32h523__',
    'stm32h523zc': 'stm32h523__',
    'stm32h523ze': 'stm32h523__',
    'stm32h533ce': 'stm32h533__',
    'stm32h533he': 'stm32h533__',
    'stm32h533re': 'stm32h533__',
    'stm32h533ve': 'stm32h533__',
    'stm32h533ze': 'stm32h533__',
    'stm32h562ag': 'stm32h562__',
    'stm32h562ai': 'stm32h562__',
    'stm32h562ig': 'stm32h562__',
    'stm32h562ii': 'stm32h562__',
    'stm32h562rg': 'stm32h562__',
    'stm32h562ri': 'stm32h562__',
    'stm32h562vg': 'stm32h562__',
    'stm32h562vi': 'stm32h562__',
    'stm32h562zg': 'stm32h562__',
    'stm32h562zi': 'stm32h562__',
    'stm32h563ag': 'stm32h563__',
    'stm32h563ai': 'stm32h563__',
    'stm32h563ig': 'stm32h563__',
    'stm32h563ii': 'stm32h563__',
    'stm32h563mi': 'stm32h563__',
    'stm32h563rg': 'stm32h563__',
    'stm32h563ri': 'stm32h563__',
    'stm32h563vg': 'stm32h563__',
    'stm32h563vi': 'stm32h563__',
    'stm32h563zg': 'stm32h563__',
    'stm32h563zi': 'stm32h563__',
    'stm32h573ai': 'stm32h573__',
    'stm32h573ii': 'stm32h573__',
    'stm32h573mi': 'stm32h573__',
    'stm32h573ri': 'stm32h573__',
    'stm32h573vi': 'stm32h573__',
    'stm32h573zi': 'stm32h573__',
}
foreach mcu, type : mcus
  set_variable(f'st_cmsis_@mcu@_dep', declare_dependency(
    version: meson.project_version(),
    dependencies: get_variable(f'st_cmsis_@type@_dep'),
    link_args: ['-Xlinker', '--default-script=' + files('ld' / mcu + '.ld')[0].full_path()],
  ))
endforeach