ECサイトのデータベースでは、トランザクション処理や在庫管理、価格計算などの複雑な操作を行うことが多いです。こうした操作にストアドプロシージャを使用すると、パフォーマンスの向上やコードの再利用性、セキュリティの向上を図ることができます。

以下は、ECサイトのデータベースでのストアドプロシージャの使用例をいくつか紹介します：

注文の処理
新しい注文が入ったときに、在庫を減少させ、注文詳細を保存し、必要に応じて他の関連テーブルを更新する操作を一連のトランザクションとして処理することができます。
sql
Copy code
CREATE PROCEDURE ProcessOrder(IN orderId INT, IN productId INT, IN quantity INT)
BEGIN
    DECLARE availableStock INT;

    -- 在庫をチェック
    SELECT stock INTO availableStock FROM products WHERE id = productId;
    IF availableStock < quantity THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Not enough stock';
    ELSE
        -- 在庫を減少させる
        UPDATE products SET stock = stock - quantity WHERE id = productId;

        -- 注文詳細を保存
        INSERT INTO order_details (order_id, product_id, quantity) VALUES (orderId, productId, quantity);
    END IF;
END;
商品の合計価格の取得
カート内の商品の合計価格を計算するためのストアドプロシージャ。
sql
Copy code
CREATE PROCEDURE GetTotalCartPrice(IN cartId INT, OUT total DECIMAL(10,2))
BEGIN
    SELECT SUM(p.price * c.quantity)
    INTO total
    FROM cart_items c
    JOIN products p ON c.product_id = p.id
    WHERE c.cart_id = cartId;
END;
特定のカテゴリのベストセラー商品の取得
特定のカテゴリに属する商品の中で、最も多く売れている商品を取得するストアドプロシージャ。
sql
Copy code
CREATE PROCEDURE GetBestSellingProduct(IN categoryId INT, OUT productId INT)
BEGIN
    SELECT p.id 
    INTO productId
    FROM products p
    JOIN order_details o ON p.id = o.product_id
    WHERE p.category_id = categoryId
    ORDER BY SUM(o.quantity) DESC
    LIMIT 1;
END;
上記の例は基本的なものですが、実際のECサイトではもっと複雑なロジックや多くのテーブルとの相互作用が必要になることが考えられます。ストアドプロシージャを適切に使用することで、これらの操作を効率的に、セキュアに行うことができます。
