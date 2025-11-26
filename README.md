# 🚀 Guia de Configuração Frontend + Backend

## 1️⃣ Configurar o Backend Django

### Instalar dependências
```bash
pip install -r requirements.txt
```

### Criar variáveis de ambiente (.env)
```bash
SECRET_KEY=django-insecure-sua-chave-secreta-aqui
DEBUG=True
ALLOWED_HOSTS=127.0.0.1,localhost
```

### Migrar banco de dados
```bash
python manage.py migrate
```

### Criar superuser (admin)
```bash
python manage.py createsuperuser
# username: admin
# password: sua-senha
```

### Iniciar servidor Django
```bash
python manage.py runserver 0.0.0.0:8000
```

✅ Django estará em: **http://127.0.0.1:8000**

---

## 2️⃣ Criar um Post de Teste

### Opção A: Criar via Django Admin
1. Acesse: http://127.0.0.1:8000/admin/
2. Faça login com o superuser
3. Clique em "Posts" → "Adicionar Post"
4. Preencha com:
   - **Autor**: seu usuário
   - **Mensagem**: "Teste de postagem"
   - **Aprovado**: ✅ Marque
5. Clique em "Salvar"

### Opção B: Criar via Django Shell
```bash
python manage.py shell
```

```python
from django.contrib.auth.models import User
from Farmacia.models import Post

user = User.objects.first()  # ou seu usuário
post = Post.objects.create(
    autor=user,
    mensagem="Teste de postagem",
    aprovado=True
)
print(f"Post criado: {post.id}")
exit()
```

### Opção C: Usar a API
```bash
curl -X GET http://127.0.0.1:8000/api/posts/
```

Você deve ver algo como:
```json
[
  {
    "id": 1,
    "autor": "admin",
    "mensagem": "Teste de postagem",
    "imagem": null,
    "imagem_url": null,
    "data_postagem": "2025-01-20T10:30:00Z",
    "aprovado": true
  }
]
```

---

## 3️⃣ Configurar o Frontend

### Opção A: Usar Live Server (VS Code)
1. Instale extensão "Live Server" no VS Code
2. Abra `index.html`
3. Clique em "Go Live" (canto inferior direito)
4. Abrirá em: **http://127.0.0.1:5500** (ou similar)

### Opção B: Servir arquivos estáticos
```bash
# Na pasta do frontend
python -m http.server 8080
```

Acesse: **http://127.0.0.1:8080**

---

## 4️⃣ Testar Comunicação

### Verificar no Console do Navegador
1. Abra `http://127.0.0.1:5500` (ou porta do seu frontend)
2. Aperte **F12** → Abrir Developer Tools
3. Vá em **Console**
4. Você deve ver:
   - ✅ "Posts recebidos:" com os dados
   - ✅ Posts renderizados na página
   - ❌ Se houver erro de CORS, verificar seção de troubleshooting

---

## 5️⃣ Checklist de Verificação

### Backend Django
- [ ] Servidor rodando em `http://127.0.0.1:8000`
- [ ] Admin acessível em `http://127.0.0.1:8000/admin/`
- [ ] Pelo menos 1 post criado e aprovado
- [ ] API retorna posts: `http://127.0.0.1:8000/api/posts/`
- [ ] CORS habilitado no `settings.py`

### Frontend
- [ ] Arquivos `index.html` em uma pasta separada
- [ ] Arquivo `css/estilo.css` no mesmo diretório
- [ ] Página carrega sem erros
- [ ] Console mostra "Posts recebidos:"
- [ ] Posts aparecem na página

---

## 🔧 Troubleshooting

### ❌ Erro de CORS
**Problema:** `Access to XMLHttpRequest blocked by CORS policy`

**Solução:** Verificar `aula03/settings.py`:
```python
CORS_ALLOWED_ORIGINS = [
    "http://127.0.0.1:8000",
    "http://localhost:8000",
    "http://127.0.0.1:5500",
    "http://localhost:5500",
]
```

Reiniciar Django após salvar.

### ❌ Posts não aparecem
1. Verificar no admin se posts existem
2. Verificar se `aprovado = True`
3. Abrir Console (F12) e procurar erros
4. Testar API diretamente: `http://127.0.0.1:8000/api/posts/`

### ❌ Imagens não carregam
1. Verificar se arquivo foi enviado
2. Verificar pasta `uploads/posts/`
3. Testar URL da imagem no navegador

### ❌ Django retorna 404
Verificar se arquivo `.env` existe e `SECRET_KEY` está definida.

---

## 📱 Estrutura de Pastas Recomendada

```
meu-projeto/
├── backend/
│   ├── aula03/
│   ├── Farmacia/
│   ├── contas/
│   ├── manage.py
│   ├── db.sqlite3
│   ├── .env
│   └── requirements.txt
│
└── frontend/
    ├── index.html
    ├── css/
    │   └── estilo.css
    └── postar/
        └── index.html
```

---

## 🎯 Próximos Passos

1. ✅ Confirmar que posts aparecem
2. ⭕ Adicionar formulário de cadastro no frontend
3. ⭕ Implementar autenticação JWT
4. ⭕ Deploy em produção
