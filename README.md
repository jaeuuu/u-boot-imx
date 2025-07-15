## jig_board

### 1. Watch Dog bring-up
- PWM1 인터페이스 사용.
- u-boot-imx 에는 기본적으로 PWM에 대한 지원이 되어 있지 않음. (imx5, 6 제외)
- 직접 PWM clock, register 등을 제어해 PWM을 활성화시켜야 함.
- <arch/arm/include/asm/arch-imx8m/imx-reg.h> 에 PWM registers 구조체 정의.
- <arch/arm/include/asm/arch-imx8m/clock.h> 에 PWM clock 제어 관련 함수 정의.
- <arch/arm/mach-imx/imx8m/clock_imx8mm.c>에 PWM clock 제어 관련 함수 정의. (imx8mm, imx8mp 공통 소스 트리를 사용하는듯 함.)
- 별도의 클럭 framework가 없기 때문에 <drivers/pwm/pwm-imx-utils.c> 에 사용 중인 define 값 정의.
```
#define CONFIG_IMX6_PWM_PER_CLK		66000000
```
- <board/freescale/imx8mp_evk/imx8mp_evk.c> 에 setup_ext_wdog_strobe() 함수 정의
- CONFIG_PWM_IMX=y

### 2. CPLD bring-up
- CPLD를 통해 Master/Slave 결정을 위한 함수 및 로직 추가.
- <board/freescale/imx8mp_evk/imx8mp_evk.c> 에 setup_cpld() 함수 정의
- 현재는 CPLD로 부터 직접 읽는 ID값이 없기 때문에 일부 GPIO를 사용해 Master/Slave를 판단토록 함.
- 현재 CPLD를 수동 reset 해주어야 하기 때문에 10초 delay 존재. 추후 삭제 필요.