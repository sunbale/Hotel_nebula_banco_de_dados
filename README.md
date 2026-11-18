# Hotel_nebula_banco_de_dados

O Hotel nébula tinha um grande problema: Muitos dados que precisavam ser organizados e nenhum local para armazená-los, então foi criado um banco de dados para armazenar todos os serviços que eram utilizados. O banco de dados funciona com a seguinte hierarquia:

HOSPEDES -> RESERVAS -> HOSPEDAGENS -> PAGAMENTOS/FEEDBACKS
QUARTOS, FUNCIONÁRIOS, SERVIÇOS E FEEDBACKS

Para deixar mais preciso, temos estes relacionamentos:
Origem	Referência	Finalidade
*Reservas*	_hospede_id_	Identificar o cliente
*Reservas*	_quarto_id_	Identificar o quarto
*Hospedagens*	_reserva_id_	Identificar a reserva
*Hospedagens*	_hospede_id_	Identificar o cliente
*Hospedagens*	_quarto_id_	Identificar o quarto
*Hospedagens*	_funcionario_checkin_id_	Identificar quem realizou o check-in
*Hospedagens*	_funcionario_checkout_id_	Identificar quem realizou o checkout
*Hospedagens*	_servico_id_	Identificar o serviço consumido
*Pagamentos*	_hospedagem_id_	Identificar a hospedagem
*Pagamentos*	_reserva_id_	Identificar a reserva
*Pagamentos*	_hospede_id_	Identificar o cliente
*Feedbacks*	_hospede_id_	Identificar quem avaliou
*Feedbacks*	_hospedagem_id_	Identificar a estadia avaliada
*Feedbacks*	_quarto_id_	Identificar o quarto avaliado
*Feedbacks*	_funcionario_id_	Identificar quem respondeu


## COMO UTILIZAR O BANCO DE DADOS
Primeiro passo: baixe os arquivos
segundo passo: importe-os no mongo db
terceiro passo: faça consultas com os seguintes códigos para garantir que tudo está funcionando como deveria:]
Exemplo: encontrar todos os quartos que possuem banheira.
db.Quartos.find({
    "caracteristicas.banheira": true
})
Encontrar quartos com vista para o mar (Não temos por enquanto)
db.Quartos.find({
    "caracteristicas.vista": "mar"
})
Encontrar quartos com a categoria Luxo:
db.quartos.find({
    categoria: "luxo"
})

Encontrar hospedes que reservaram mais de uma vez(Não temos ainda):
db.reservas.aggregate([
    {
        $group: {
            _id: "$hospede_id",
            total_reservas: {
                $sum: 1
            }
        }
    },
    {
        $match: {
            total_reservas: {
                $gt: 1
            }
        }
    }
])

faturamento do hotel:

db.Pagamentos.aggregate([
    {
        $match: {
            status: "pago",
            data_pagamento: {
                $gte: "2025-04-01T00:00:00Z",
                $lt: "2025-05-01T00:00:00Z"
            }
        }
    },
    {
        $group: {
            _id: null,
            faturamento_total: {
                $sum: "$valor_total"
            }
        }
    }
])

(por enquanto o faturamento total é  4265)
