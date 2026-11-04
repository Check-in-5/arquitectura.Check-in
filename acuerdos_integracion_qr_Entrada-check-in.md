# Acuerdos de Integración: Formato de Código QR y Backend

## Formato de Codificación Sugerido

Texto plano en formato JSON estandarizado dentro del código QR (sin encriptación).

## Estructura de Datos Requerida

El contenido decodificado del código QR deberá respetar exactamente la siguiente estructura JSON:

```json
{
  "id_entrada": "550e8400-e29b-41d4-a716-446655440000",
  "id_usuario": 10293,
  "id_evento": 5
}
```

## Especificación Técnica de Campos

Indicaciones para el Equipo de Entradas respecto a los valores esperados:

| Campo | Tipo | Descripción | 
| ----- | ----- | ----- | 
| `id_entrada` | String (UUID v4) | Operará como identificador único para consultar la tabla en la base de datos MySQL y validar el estado de acceso. Previene la duplicidad y el doble check-in. | 
| `id_usuario` | Integer | Requerido según la actualización de campos implementada en el backend (`service.js`). Relaciona físicamente el ticket escaneado con la entidad del usuario. | 
| `id_evento` | Integer | Permite al backend validar si el ticket corresponde al evento activo configurado en el sistema local. | 

## Flujo de Integración de los Sistemas

El ciclo de vida de la lectura de un código QR será el siguiente:

1. El dispositivo escáner captura el código QR en la puerta y extrae el texto JSON.
2. El cliente envía este JSON al endpoint correspondiente del backend Node.js.
3. El controlador ejecuta la consulta asíncrona en la base de datos MySQL, verificando el estado de `id_entrada` y su relación con el `id_usuario`.
4. El sistema actualiza el registro en la base de datos a "utilizado" y retorna la respuesta HTTP de éxito o error al dispositivo.