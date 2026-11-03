# BCSentinel -- Registro publico de anclaje de pruebas

Cada fila de abajo prueba que, en esa fecha, ya existia una cadena de sellos criptograficos (SHA-256, hash-chain, ver `case_sealing.py` en el repositorio principal) sobre casos que BCSentinel vigila internamente -- **sin revelar que wallets o formaciones son**, solo que 'algo concreto' estaba sellado en ese momento exacto.

Cuando uno de esos casos se haga publico, compartiremos los datos originales sellados de ESE caso en concreto, y cualquiera -- sin tener que confiar en nuestra palabra -- puede recalcular el hash y comprobar que coincide con un `chain_hash` ya publicado aqui, en una fila con fecha ANTERIOR a que el caso se hiciera publico.

| Fecha del anclaje (UTC) | Total de sellos | Ultimo chain_hash |
|---|---|---|
| 2026-11-03 10:50 UTC | 1 | `8327850276fa5f5fbcdc87d06a91565bf66460cece0bdfd1949fca25bc432836` |
