# Bookstore — M57

Neste módulo implementei os serializers de Category, Product e Order com Django REST Framework. A categoria aparece nos dados do produto, e o pedido apresenta seus produtos com as respectivas categorias.

Para cadastrar ou alterar um produto, o serializer recebe `category_id`. Para cadastrar ou alterar um pedido, recebe `user` e `product_ids`. Os dados de categoria e produtos são retornados de forma aninhada, sem exigir a criação de novos registros relacionados.

## Modelos

- Category: nome obrigatório e único; descrição opcional.
- Product: nome, preço com duas casas decimais, categoria, descrição opcional e indicador de ativo. O preço mínimo é 0,01.
- Order: usuário, produtos, situação e data de criação. A situação pode ser pending, completed ou cancelled.

Um produto pertence a uma categoria. Um pedido pertence a um usuário e pode conter vários produtos. A lista de produtos enviada ao serializer não pode ser vazia nem conter IDs repetidos. Essa última validação evita que a relação muitos-para-muitos remova repetições silenciosamente. Quantidade e itens de pedido não fazem parte deste exercício.

## Executar

```powershell
poetry install --no-root
poetry run python manage.py migrate
poetry run python manage.py runserver
```

O projeto utiliza SQLite para desenvolvimento. A página inicial do Django permanece em http://127.0.0.1:8000/. Não foram criadas rotas REST neste módulo, pois a atividade solicita serializers e testes.

## Testes

```powershell
poetry run python manage.py test --verbosity 2
```

A suíte contém 25 testes com `django.test.TestCase`, cobrindo dados válidos, campos obrigatórios, valores inválidos, campos retornados, relacionamentos, criação, atualização parcial e campos somente de leitura. O Django utiliza um banco separado para os testes.

## Estrutura principal

- `api/models.py`: modelos Category, Product e Order.
- `api/serializers.py`: conversão e validação dos dados.
- `api/tests.py`: testes automatizados.
- `api/migrations/0001_initial.py`: criação das tabelas.

## Referências de estudo

- Apostila do módulo: Integrando Modelos e Serializers em Django Rest Framework.
- Material introdutório: https://gist.github.com/havokkmorands/f7227a2d4c732eafb80f1d362b6d4bad
- Documentação: https://www.django-rest-framework.org/api-guide/serializers/
