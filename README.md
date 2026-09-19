# Projeto Aplicado: Práticas de Mercado - Pós-Graduação Lato Sensu - Segurança da Informação e Análise Forense - UNCISAL

# ## 🔒 Infraestrutura, Criptografia e Segurança

Esta secção documenta a arquitetura de segurança implementada na instância de produção AWS EC2, detalhando a emissão de certificados SSL/TLS para endereço IP público, mitigação de ameaças com criptografia pós-quântica (PQC), endurecimento (*hardening*) de cabeçalhos HTTP e defesa ativa contra ataques de força bruta.

### 1. Certificado SSL/TLS Let's Encrypt para IP Público

* **Endereço IP:** `18.231.175.123`
* **Autoridade Certificadora:** Let's Encrypt (CA Intermediária: `YE2`)
* **Validade:** Curto prazo (`shortlived`, 7 dias com renovação automática configurada via Certbot)
* **SAN (*Subject Alternative Name*):** Endereço IP `18.231.175.123`

#### Evidência:
<!-- Arraste e solte aqui o print do visualizador do certificado (aquele que mostra Let's Encrypt e a validade de 19 a 26 de setembro de 2026) -->

### 2. Suporte a Criptografia Pós-Quântica (PQC) e TLS 1.3

O servidor Nginx foi configurado para operar sobre **TLSv1.2 e TLSv1.3**, priorizando a negociação do grupo de troca de chaves híbrido pós-quântico **`X25519MLKEM768`** (combinação do algoritmo clássico X25519 com o padrão NIST ML-KEM/Kyber768), protegendo o tráfego contra futuras ameaças de computação quântica.

#### Configuração aplicada no Nginx:
```nginx
ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers off;
ssl_ecdh_curve X25519MLKEM768:X25519:prime256v1:secp384r1;

