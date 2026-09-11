<div align="center">

[![English](https://img.shields.io/badge/README-English-blue)](README.md)
[![Русский](https://img.shields.io/badge/README-Русский-red)](README.ru.md)
[![Oʻzbekcha](https://img.shields.io/badge/README-Oʻzbekcha-green)](README.uz.md)

</div>

# Руководство по диагностике VLAN
[![CI](https://github.com/uMax-Cyber/VLANscope/actions/workflows/ci.yml/badge.svg)](https://github.com/uMax-Cyber/VLANscope/actions/workflows/ci.yml)


![Демонстрация](screenshots/demo.svg)
Практическое руководство по диагностике проблем с VLAN в мультивендорных средах (коммутаторы UniFi, межсетевые экраны Sophos, Linux-мосты). Рассматриваются обнаружение несоответствий native/tagged VLAN, проверка trunk-портов и отладка DHCP между VLAN.

## ✨ Возможности

- **Обнаружение несоответствий native/tagged VLAN** — поиск портов, где native VLAN не совпадает с untagged-трафиком
- **Проверка trunk-портов** — подтверждение, какие VLAN реально проходят через trunk
- **Отладка DHCP между VLAN** — проверка relay и правил файрвола для UDP 67/68
- **Мультивендорное покрытие** — UniFi, Sophos и Linux-мосты в одном руководстве
- **Готовые команды диагностики** — рецепты `tcpdump` / `ip` для копирования

## Типичные проблемы VLAN (и их решения)

### 1. Несоответствие native VLAN
**Симптом**: устройства получают IP-адреса из чужой подсети. Связь периодически пропадает.
**Причина**: native VLAN на порту коммутатора ≠ ожидания устройства.
**Решение**: проведите аудит всех портов — native VLAN должен совпадать с untagged-трафиком на этом порту.

### 2. DHCP не проходит между VLAN
**Симптом**: устройства в одном VLAN не могут получить DHCP от сервера в другом.
**Причина**: либо не настроен DHCP relay, либо файрвол блокирует UDP 67/68 между VLAN.
**Решение**: проверьте конфигурацию relay на шлюзе; проверьте правила файрвола для портов DHCP.

### 3. Trunk не передаёт теги VLAN
**Симптом**: одни VLAN работают, другие нет — по одному и тому же физическому линку.
**Причина**: trunk-порт не разрешает все нужные VLAN (tagged_vlan_mgmt = block_all).
**Решение**: явно разрешите нужные VLAN на trunk-портах.

### 4. Статическая резервация не в той области
**Симптом**: одно конкретное устройство зависает на «connecting…», пока остальные работают.
**Причина**: у устройства статическая DHCP-резервация в области VLAN-A, а подключается оно к VLAN-B.
**Решение**: перенесите резервацию в правильную область (или сделайте её глобальной).

## Инструменты диагностики

```bash
# Проверить, какие VLAN реально есть на trunk
tcpdump -lni <iface> vlan -c 20

# Проверить путь DHCP relay
# На шлюзе: проверить конфигурацию DHCP relay для каждого VLAN-интерфейса

# Тест из конкретного VLAN (создать временный субинтерфейс)
ip link add link eth0 name eth0.XXX type vlan id XXX
ip link set eth0.XXX up
dhclient eth0.XXX
```

## Лицензия
MIT

## 📬 Контакты

Вопросы? Пишите: **[allumaxmail@gmail.com](mailto:allumaxmail@gmail.com)**

---

<div align="center">

[![English](https://img.shields.io/badge/README-English-blue)](README.md)
[![Русский](https://img.shields.io/badge/README-Русский-red)](README.ru.md)
[![Oʻzbekcha](https://img.shields.io/badge/README-Oʻzbekcha-green)](README.uz.md)

</div>
