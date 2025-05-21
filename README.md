# meson-embed 

Ports of projects used in embedded software development for the Meson build system.

_This project is in its early stages, any feedback would be helpful.  Contribution is very welcome._

## Goals

- [x] stm32 single-core nonsecure cross files
- [ ] [st-cmsis](https://github.com/STMicroelectronics/STM32Cube_MCU_Overall_Offer?tab=readme-ov-file#stm32cube-cmsis) mcu with simple project structre (c0 f0 f1 f2 f3 f4 f7 g0 g4 l0 l1 l4 u0 u3 wb wb0 wba wl3) _WIP_
- [x] [st-cmsis-core](https://github.com/STMicroelectronics/cmsis-core) core headers
- [ ] [st-hal](https://github.com/STMicroelectronics/STM32Cube_MCU_Overall_Offer?tab=readme-ov-file#stm32cube-hal-drivers)
- [ ] [libopencm3](https://github.com/libopencm3/libopencm3)
- [ ] [tinyusb](https://github.com/hathach/tinyusb)
- [ ] [lwip](https://git.savannah.gnu.org/cgit/lwip.git/)
- [ ] [u8g2](https://github.com/olikraus/u8g2)
- [ ] [scpi](https://github.com/j123b567/scpi-parser)
- [ ] [eyalroz/printf](https://github.com/eyalroz/printf)
- [ ] [stm32duino](https://github.com/stm32duino/Arduino_Core_STM32)
- [ ] stm32 multi-core and|or secure cross files
- [ ] [st-cmsis](https://github.com/STMicroelectronics/STM32Cube_MCU_Overall_Offer?tab=readme-ov-file#stm32cube-cmsis) mcu with complex project structre (h5 h7 h7rs l5 n6 u5 wl)

## Scripts for porting

Large projects require some automation. Python scripts in the `scripts` directory are used for them.

## References

Some great examples:
 * https://jonathanhamberg.com/post/2020-01-30-stm32-meson/
 * https://git.sr.ht/~kdsch/libopencm3
 * https://github.com/LunarLambda/meson-gba
 * https://github.com/FlyingBBQ/meson_embedded

ST CMSIS and HAL with CMake:
 * https://github.com/ObKo/stm32-cmake/

Helpful information about stm32 MCUs:
 * https://www.st.com/content/st_com/en/stm32-mcu-developer-zone/mcu-portfolio.html
 * https://gist.github.com/cpq/3812a8a542b3ce31bd9ce89a24816b7b
 * https://community.st.com/t5/stm32-mcus-wireless/min-heap-size-and-min-stack-size/td-p/184106
