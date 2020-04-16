# htmlとSlimの比較

## html
```html
<!DOCTYPE html>
<html>
<head>
  <title>Slim Examples</title>
  <meta name="keywords" content="template language">
  <meta name="author">
  <script>
    alert('Slim supports embedded javascript!');
  </script>
</head>

<body>
  <h1>Markup examples</h1>
  <div id="content">
    <p>This example shows you what a basic Slim file looks like.</p>

    <%= yield %>

    <% unless items.empty? %>
      <table>
        <% items.each do |item| %>
          <tr>
            <td class="name">
              <%= item.name %>
            </td>
            <td class="price">
              <%= item.price %>
            </td>
          </tr>
        <% end %>
      </table>
      <% else %>
        <p>No items found. Please add some inventory.Thank you!</p>
      <% end %>
  </div>
  <div id="footer">
    <%= render 'footer' %>
    Copyright © <%= year %><%= author %>
  </div>
</body>
</html>
```

## slim
同じ内容を *Slim* で書き表すと次のようになる。
```html
doctype html
html
  head
    title Slim Examples
    meta name="keywords" content="template language"
    meta name="author" content=author
    javascript:
      alert('Slim supports embedded javascript!')

  body
    h1 Markup examples

    #content
      p This example shows you what a basic Slim file looks like.

      == yield

      - unless items.empty?
        table
          - items.each do |item|
            tr
              td.name = item.name
              td.price = item.price
      - else
        p
         | No items found.  Please add some inventory.
           Thank you!

    div id="footer"
      = render 'footer'
      | Copyright © #{year} #{author}
```

比較すると、開始タグに用いられていた```<>```が取れ、終了タグ```</>```が省略されるなど、すっきりしている。
```html2slim``` を 用いると、既存のhtmlソースファイルを、slimに変換できる。
