# Navigation
Sử dụng `<Link>`  hoặc hoặc `useNavigate` để thay đổi url

```
<Link to='/'>
```

```
import { useNavigate } from "react-router-dom";

const navigate = useNavigate();

navigate('/');
```

# Reading URL Parameters

Sử dụng `:style` syntax trong thẻ `<Route>` để truyề và `useParams()` để nhận params

```
<Route path="invoices/:invoiceId" element={<Invoice />} />
```

```
import { useParams } from "react-router-dom";

const Invoice = () => {
  let params = useParams();
  return <h1>Invoice {params.invoiceId}</h1>;
}
```
`Chú ý: ` path segment `:invoiceId` và param's key `params.invoiceId` phải tương ứng với nhau.

# Nested Routes