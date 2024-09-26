# Проектная работа "Веб-ларек"

Стек: HTML, SCSS, TS, Webpack

Структура проекта:
- src/ — исходные файлы проекта
- src/components/ — папка с JS компонентами
- src/components/base/ — папка с базовым кодом

Важные файлы:
- src/pages/index.html — HTML-файл главной страницы
- src/types/index.ts — файл с типами
- src/index.ts — точка входа приложения
- src/styles/styles.scss — корневой файл стилей
- src/utils/constants.ts — файл с константами
- src/utils/utils.ts — файл с утилитами

## Установка и запуск
Для установки и запуска проекта необходимо выполнить команды

```
npm install
npm run start
```

или

```
yarn
yarn start
```
## Сборка

```
npm run build
```

или

```
yarn build
```

# Основные компоненты

### BasketModel
Модель корзины, управляющая добавлением и удалением товаров, а также оповещающая представление об изменениях через события.

```typescript
class BasketModel {
    items: Map<string, number>;
    constructor(protected events: IEventEmitter) {}

    add(id: string): void {
        this.items.set(id, (this.items.get(id) || 0) + 1);
        this._changed();
    }

    remove(id: string): void {
        this.items.delete(id);
        this._changed();
    }

    protected _changed(): void {
        this.events.emit('basket:change', { items: Array.from(this.items.keys()) });
    }
}
```
### CatalogModel
Модель каталога, предоставляющая методы для получения списка продуктов.

```typescript
class CatalogModel {
    items: IProduct[] = [];
    setItems(items: IProduct[]): void {
        this.items = items;
    }

    getProduct(id: string): IProduct {
        return this.items.find(item => item.id === id);
    }
}
```

### EventEmitter
Система управления событиями, обеспечивающая связь между компонентами через события.

```typescript
class EventEmitter {
    private events: { [key: string]: Function[] } = {};

    emit(event: string, data?: unknown): void {
        (this.events[event] || []).forEach(callback => callback(data));
    }

    on(event: string, callback: Function): void {
        if (!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(callback);
    }
}
```

### BasketItemView
Отображение товаров в корзине с возможностью добавления и удаления товаров.

```typescript
class BasketItemView {
    constructor(protected container: HTMLElement, protected events: IEventEmitter) {}

    render(data: { id: string, title: string }): HTMLElement {
        // логика отображения товара
    }
}
```

# Взаимодействие компонентов

BasketModel и CatalogModel управляют данными.

EventEmitter связывает компоненты через события, такие как basket:change и ui:basket-add.

BasketItemView отвечает за отображение содержимого корзины и взаимодействие пользователя с товарами.

# Паттерны проектирования

Observer (Наблюдатель): Используется для связи компонентов через события.

MVVM: Применяется для разделения модели данных и представления.


# Используемые типы данных

### IProduct
Описывает товар, включает id и title.

```typescript
interface IProduct {
    id: string;
    title: string;
}
```

# IBasketModel

```typescript
interface IBasketModel {
    items: Map<string, number>;
    add(id: string): void;
    remove(id: string): void;
}
```
