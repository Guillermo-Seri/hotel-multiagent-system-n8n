# Sistema Multi-Agente de Atención Hotelera

Proyecto final del curso **AI Automation Avanzado** de Coderhouse.

**Autor:** Guillermo Seri
**Cohorte:** 102005
**Año:** 2026

---

## Video Demo

[Ver demo de 3 minutos]((https://youtu.be/HEz_ljEYmlU?si=X1pBehcKU7ZOV2HL))

---

## Bot de prueba

Probá el sistema en Telegram: [@Hotel_Playa_bot](https://t.me/Hotel_Playa_bot)

Mensajes sugeridos:

| Mensaje                                                                                        | Worker                |
| ---------------------------------------------------------------------------------------------- | --------------------- |
| `¿Cuál es el horario del check-in?`                                                            | Atención (RAG)        |
| `¿Cuánto sale una Premium para 3 personas del 15 al 17 de octubre?`                            | Disponibilidad        |
| `Quiero reservar una Superior para 2 personas del 15 al 17 de octubre a nombre de Juan Pérez.` | Reserva               |
| `¿Cómo puedo pagar?`                                                                           | Facturación           |
| `El aire acondicionado no funciona.`                                                           | Atención (queja)      |
| `Me cobraron dos veces la reserva.`                                                            | Facturación (reclamo) |

---

## Contenido del repositorio

---

## Arquitectura

Sistema multi-agente sobre n8n con patrón Manager-Worker:

- **1 Manager orquestador** — recibe, clasifica y deriva.
- **4 Workers** — Disponibilidad, Reserva, Facturación, Atención.
- **Judge + Action Gate** en cada Worker — control de seguridad.
- **Notificaciones Slack** — 4 canales por tipo de evento.
- **Trazabilidad** por `traceId` end-to-end.

Detalle completo en el [DMSA](docs/DMSA-Guillermo-Seri.pdf).

---

## Stack

- n8n (self-hosted)
- Groq `openai/gpt-oss-120b` + NVIDIA Nemotron (fallback)
- Google Sheets (base de datos)
- Pinecone Assistant (RAG)
- Telegram Bot API (canal)
- Slack Web API (notificaciones)

---

## Instalación

1. **Importar los 5 JSON** en n8n (Workflows → Import from File).
2. **Configurar credenciales** al importar (n8n pedirá reasignar cada una):
   - Google Sheets OAuth2
   - Groq API
   - NVIDIA NIM API
   - OpenRouter API
   - Pinecone API
   - Telegram Bot Token
   - Slack Bot Token
3. **Configurar Google Sheets** con las hojas: `DISPONIBILIDAD`, `TARIFAS`, `HABITACIONES`, `RESERVAS`, `INFO-PAGOS`.
4. **Configurar Pinecone** con el índice `hotel-playa-faq` (768 dim, cosine) y subir el manual de `data/`.
5. **Activar los workflows**.

Los IDs de credenciales dentro de los JSON son referencias internas del entorno original. No contienen secretos. n8n te pedirá reasignarlos al importar.

---

## Documentación completa

El informe completo (arquitectura, casos de prueba, ROI) está en:

**[DMSA-Guillermo-Seri.pdf](docs/DMSA-Guillermo-Seri.pdf)**

---

## Licencia

Proyecto académico — Coderhouse 2026.
