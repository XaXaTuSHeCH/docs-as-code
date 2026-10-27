# Диаграммы

## Диаграмма последовательности

Показан основной поток обработки обращения и альтернативный путь для результата с низкой уверенностью.

<div class="mermaid">

sequenceDiagram
actor Requester as Заявитель
participant System as Система поддержки
participant Model as ML-классификатор
participant Operator as Оператор
participant Team as Группа специалистов

Requester->>System: Отправить текст обращения
System->>System: Проверить и сохранить обращение
System->>Model: Передать текст обращения
Model-->>System: Вернуть категорию и уверенность
alt Уверенность достаточная
    System->>System: Определить группу по категории
    System->>Team: Назначить обращение
    System-->>Requester: Показать номер и статус
else Низкая уверенность или ошибка обработки
    System->>Operator: Передать в очередь ручной классификации
    Operator->>System: Выбрать категорию и группу
    System->>Team: Назначить обращение
    System-->>Requester: Показать номер и статус проверки
end

</div>

## Диаграмма классов

<div class="mermaid">

classDiagram
class Requester {
+int id
+string contact
}

class SupportRequest {
+int id
+string text
+string status
+datetime createdAt
}

class ClassificationResult {
+string category
+float confidence
+bool requiresReview
}

class SupportGroup {
+int id
+string name
}

class SupportOperator {
+int id
+string name
+reviewClassification()
}

Requester "1" --> "*" SupportRequest : creates
SupportRequest "1" --> "0..1" ClassificationResult : receives
ClassificationResult "*" --> "0..1" SupportGroup : routes to
SupportOperator "1" --> "*" SupportRequest : reviews

</div>
