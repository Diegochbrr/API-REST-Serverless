# Workshop Serverless en AWS: CRUD de TODOs con AWS SAM

API REST Serverless desplegada en AWS utilizando **Infraestructura como Código (IaC)** con AWS SAM, Amazon API Gateway, AWS Lambda (Python 3.13) y Amazon DynamoDB.

## Arquitectura y Componentes
- **API Gateway (`TodosApi`)**: Expone los endpoints REST bajo el stage `dev` con soporte CORS.
- **AWS Lambda (Python 3.13)**: Cinco funciones independientes (`CreateTodoFunction`, `ListTodosFunction`, `GetTodoFunction`, `UpdateTodoFunction`, `DeleteTodoFunction`) que comparten utilidades mediante `src/utils.py`.
- **Amazon DynamoDB (`TodosTable`)**: Tabla NoSQL en modo `PAY_PER_REQUEST` con clave de partición `id` (String).

## Qué aprendí en este taller
1. **Infraestructura como Código (IaC)**: Definir toda una arquitectura serverless en `template.yaml` sin crear recursos manualmente en la consola, automatizando la creación de roles IAM, permisos de invocación y despliegue de API Gateway.
2. **Principio de menor privilegio**: Asignar políticas granulares de SAM (`DynamoDBReadPolicy` para funciones de consulta y `DynamoDBCrudPolicy` para funciones de escritura/borrado) limitadas exclusivamente al ARN de la tabla.
3. **Expresiones condicionales y de actualización en DynamoDB**: Uso de `ConditionExpression: attribute_exists(id)` en operaciones `PUT` y `DELETE` para garantizar idempotencia y retornar códigos `404` cuando un recurso no existe, además de manejar palabras reservadas como `title` mediante `ExpressionAttributeNames`.
4. **Ciclo de vida Serverless**: Flujo completo desde `sam validate`, `sam build` y `sam deploy --guided`, hasta el monitoreo de trazas en CloudWatch con `sam logs` y destrucción limpia de recursos con `sam delete`.
