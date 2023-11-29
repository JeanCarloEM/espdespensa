# esphome_timed_door_light

## Requisitos

### Etapas

![alt text](https://raw.githubusercontent.com/JeanCarloEM/esphome_timed_door_light/master/doc/illustration.svg)

#### Passo 1

Ao ser aberta a porta, a luz acende, porém, a luz permanece aberta por um período máximo `maxseconds` de segundos;

#### Passo 2

Se a porta permaneceu aberta por mais segundos que `maxseconds` então a luz é apagada;

#### Passo 3

Ao ser fechada a porta se a luz já estiver apagada não faça nada, porém se a luz ainda estiver acesa, a mesma é mantida acesa um tempo obtido atravéz da fórmula: $$T = minseconds + (add\cdot (counter - 1))$$,

Onde:

- $$minseconds \ \epsilon\ \mathbb{N}, 10 \leqslant minseconds \leqslant 1800$$
- $$add\ \epsilon\ \mathbb{N}, 10 \leqslant add \leqslant 60$$
- $$counter\ \epsilon\ \mathbb{N},\ counter >  0$$
- $$T\ \epsilon\ \mathbb{N}, T < minseconds \cdot (maxadd+1), T < maxseconds$$
- $$maxadd\ \epsilon\ \mathbb{R}, 0.1\leqslant minseconds \leqslant 7$$

#### Passo 4

No passo 4 verifica-se por `between_onoff_check` segundos se a porta voltou a ser aberta, para que a partir disso, ajustar `my__increase_off_rounds` a fim de evitar novo acendimento, conforme subtítulo [Prevenir Recorrência](#prevenir-recorrência). Note que `between_onoff_check` é contado a partir do desligamento da luz, e NÃO a partir do fechamento da porta, pois o objetivo é verifica a reincidência de regligação da luz.

### Prevenir Recorrência

Na etapa 4, `my__increase_off_rounds` é usado como um contador para medir quantas vezes houve a recorrência da abertura da porta dentro dos `between_onoff_check` segundos a partir do desligamento da luz.

Este contador deve ser zerado duas vezes ao dia.
Somente deve ser considerado recorrência >= 1.

### Condicionantes para ligar a luz

#### Apenas a noite

Se habilitado a luz acenderá apenas se for horário noturno. Não exclui outros tipos de verificações.

Esta opção deve permitir informar um valor numérico que representa a margem para menos do pôr do sol, e para mais do nascer do sol, para indicar o horário noturno. `0` dehabilita a opção.

```
noite = (
  hora_atual < ( nascerdosol + margem  )
  ||
  hora_atual > ( pordosol - margem  )
)
```

Se o horário do pôr do sol ou o horário atual não estiver válido/inicitalizado, acende a luz.

#### Com luminozidade baixa

Se habilitado a luz acenderá apenas se a luminosidade for baixa. Não exclui outros tipos de verificações.

Esta opção deve permitir informar um valor numérico (unidade lx) o teto de luminosidade permitida para ligar a luz. Valores de luminosidade menores que o valor deste, devem acender a luz. `0` dehabilita a opção.

Se o valor da luminosidade atual não for válido/inicitalizado, acende a luz.
