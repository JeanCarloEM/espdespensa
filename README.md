# esphome_timed_door_light

## Etapas

![alt text](![alt text](https://github.com/JeanCarloEM/esphome_timed_door_light/blob/master/doc/illustration.svg?raw=true))

1. No passo 1, ao ser aberta a porta, a luz acende, porém, a luz permanece aberta por um período máximo `maxseconds` segundos;

2. No passo 2, se a porta permaneceu aberta por mais segundos que `maxseconds` então a luz é apagada;

3. No passo 3, ao ser fechada a porta, E se a luz ainda estiver acesa, a mesma é mantida acesa um tempo obtido atravéz da fórmula: $$\begin{matrix} r = minseconds + (add\cdot (counter - 1)) \end{matrix} where: \begin{Bmatrix} minseconds \ \epsilon\ \mathbb{N}, 10 \leqslant minseconds \leqslant 1800\\ add\ \epsilon\ \mathbb{N}, 10 \leqslant add \leqslant 60  \\ counter\ \epsilon\ \mathbb{N},\ counter >  0\\ r\ \epsilon\ \mathbb{N}, r < minseconds \cdot (maxadd+1), r < maxseconds\\ maxadd\ \epsilon\ \mathbb{R}, 0.1\leqslant minseconds \leqslant 7 \end{Bmatrix}$$;

4. No passo 4 verifica-se por `between_onoff_check` segundos se a porta voltou a ser aberta, para que a partir disso, ajustar `my__increase_off_counter` a fim de evitar novo acendimento, conforme [anchor](#prevenir-recorrencia). Note que `between_onoff_check` é contado a partir do desligamento da luz, e NÃO a partir do fechamento da porta, pois o objetivo é verifica a reincidência de regligação da luz.

### Prevenir Recorrência

Na etapa 4, `my__increase_off_counter` é usado como um contador para medir quantas vezes houve a recorrência da abertura da porta dentro dos `between_onoff_check` segundos a partir do desligamento da luz.

```
esphome -s devicename despensa -s upper_devicename Despensa -s devicecomment "Luz automática da Despensa" -s board d1_mini -s pin_rele 13 -s pin_button 16 -s pin_sensor 10 run main.yaml
```
