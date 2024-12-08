Desenvolvimento do código para reconhecimento das placas veiculares

Este código apresenta um sistema completo para a detecção, reconhecimento e validação de placas veiculares em tempo real.
Utilizando o OpenCV, uma biblioteca de processamento de imagem de cógido aberto, combinando técnicas de processamento de imagens com um OCR avançado (Tesseract) e uma integração com uma API para verificar o registro da placa detectada. 
É ideal para aplicações como controle de acesso em estacionamentos, segurança e monitoramento de tráfego. Abaixo estão os principais componentes do código e sua explicação:

1. Detecção de placas veiculares com Haar Cascade

Utiliza-se um modelo pré-treinado haarcascade_russian_plate_number.xml, encontrado em https://github.com/opencv/opencv/blob/master/data/haarcascades/haarcascade_russian_plate_number.xml, para identificar placas de veículos em imagens capturadas pela câmera. 
O classificador Haar Cascade é baseado em características semelhantes ao Haar (Haar-like features), que examina regiões retangulares adjacentes em locais específicos dentro de uma janela de detecção.
O modelo soma as intensidades dos pixels em cada uma dessas regiões e calcula a diferença entre essas somas.
Essa diferença é então utilizada para categorizar subseções da imagem, permitindo a detecção eficiente de padrões, como placas de veículos, com alta precisão, mesmo sob condições variadas de iluminação e perspectiva.

2. Reconhecimento óptico de caracteres

Após a identicação da placa, o código usa o Tesseract OCR (versão 4.0 ou superior), essa versão utiliza um modelo baseado em LSTMs(Long Short-Term Memory), um tipo de rede neural em camadas, que foi treinada para reconhecer padrão de texto
em sequência, o que melhora o desempenho OCR (Reconhecimento Óptico de Caracteres), evitando problemas como desvanecimento e explosão de gradiente principalmente em imagens distorcidas, deste modo podemos extrair o texto da imagem capturada pela câmera.
Parâmetros como (–psm 8) são utilizados para otimizar o reconhecimento de texto, o tesseract deve interpretar a imagem como uma única linha de texto, melhorando a detecção de placas.
O texto extraído é validado por meio de expressões regulares, que verificam se a placa segue os formatos estabelecidos para os modelos de placas brasileiras, incluindo tanto o padrão antigo quanto o novo modelo do Mercosul.

3. Integração da API para Validação de Dados

O sistema consulta nossa API para verificar se a placa detectada está registrada no banco de dados gerados pelo usuários do app. Esta integração permite funcionalidades adicionais, como autenticação em tempo real e análise de permissões.

4. Controle de FPS

Para garantir que o processamento seja eficiente e adequado ao hardware utilizado, o código ajusta o número de quadros processados por segundo (FPS). Isso evita sobrecarregar o sistema enquanto mantém uma experiência fluida.

5. Interface Gráfica para Visualização 

A interface exibe a captura ao vivo com as placas detectadas por retângulos, ela exibe várias capturas de quadros para retirar todos os caracteres e quando ela valida a placa ela gera a visualização detalhada da placa, se está cadastrada na API e a
imagem detectada e processada. 
