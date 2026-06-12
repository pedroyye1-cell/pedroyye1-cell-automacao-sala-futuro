#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Script de Automação Noturna - Sala do Futuro
Responsável pela expansão e atividades automáticas durante o período noturno
"""

import schedule
import time
from datetime import datetime
import logging
from typing import List, Dict

# Configuração de logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('automacao_noturna.log'),
        logging.StreamHandler()
    ]
)
logger = logging.getLogger(__name__)


class SalaFuturoAutomacao:
    """Classe responsável pela automação da Sala do Futuro"""
    
    def __init__(self):
        self.ativo = False
        self.dispositivos = {}
        self.atividades_agendadas = []
        logger.info("Sistema de automação inicializado")
    
    def iniciar_expansao_noturna(self):
        """Inicia o modo de expansão noturna"""
        logger.info("Iniciando expansão noturna...")
        self.ativo = True
        
        # Ativar dispositivos
        self.ativar_dispositivos()
        
        # Agendar atividades
        self.agendar_atividades()
        
        logger.info("Expansão noturna ativada com sucesso")
    
    def encerrar_expansao_noturna(self):
        """Encerra o modo de expansão noturna"""
        logger.info("Encerrando expansão noturna...")
        self.ativo = False
        
        # Desativar dispositivos
        self.desativar_dispositivos()
        
        # Limpar agendamentos
        self.atividades_agendadas.clear()
        
        logger.info("Expansão noturna encerrada")
    
    def ativar_dispositivos(self):
        """Ativa dispositivos da sala"""
        dispositivos_ativos = [
            'iluminação',
            'ventilação',
            'climatização',
            'projetor',
            'sistema_som'
        ]
        
        for dispositivo in dispositivos_ativos:
            self.dispositivos[dispositivo] = True
            logger.info(f"Dispositivo ativado: {dispositivo}")
    
    def desativar_dispositivos(self):
        """Desativa dispositivos da sala"""
        for dispositivo in self.dispositivos:
            self.dispositivos[dispositivo] = False
            logger.info(f"Dispositivo desativado: {dispositivo}")
    
    def agendar_atividades(self):
        """Agenda atividades automáticas para o período noturno"""
        atividades = [
            {
                'hora': '20:00',
                'funcao': self.limpeza_automatica,
                'descricao': 'Limpeza automática da sala'
            },
            {
                'hora': '21:00',
                'funcao': self.manutencao_equipamentos,
                'descricao': 'Manutenção preventiva de equipamentos'
            },
            {
                'hora': '22:00',
                'funcao': self.diagnostico_sistemas,
                'descricao': 'Diagnóstico dos sistemas'
            },
            {
                'hora': '23:00',
                'funcao': self.relatorio_noturno,
                'descricao': 'Geração de relatório noturno'
            }
        ]
        
        for atividade in atividades:
            schedule.every().day.at(atividade['hora']).do(atividade['funcao'])
            self.atividades_agendadas.append(atividade)
            logger.info(f"Atividade agendada para {atividade['hora']}: {atividade['descricao']}")
    
    def limpeza_automatica(self):
        """Executa limpeza automática da sala"""
        if not self.ativo:
            return
        
        logger.info("Iniciando limpeza automática...")
        
        # Simulação de limpeza
        etapas = [
            'Limpeza de superfícies',
            'Desinfecção do ar',
            'Limpeza de equipamentos',
            'Verificação de higiene'
        ]
        
        for etapa in etapas:
            logger.info(f"  - {etapa} em progresso...")
            time.sleep(1)
        
        logger.info("Limpeza automática concluída!")
    
    def manutencao_equipamentos(self):
        """Executa manutenção preventiva dos equipamentos"""
        if not self.ativo:
            return
        
        logger.info("Iniciando manutenção preventiva...")
        
        equipamentos = [
            'projetor',
            'sistema_som',
            'climatização',
            'ventilação'
        ]
        
        for equipamento in equipamentos:
            logger.info(f"  - Verificando {equipamento}...")
            time.sleep(0.5)
        
        logger.info("Manutenção preventiva concluída!")
    
    def diagnostico_sistemas(self):
        """Executa diagnóstico dos sistemas"""
        if not self.ativo:
            return
        
        logger.info("Iniciando diagnóstico dos sistemas...")
        
        sistemas = {
            'energia': 'OK',
            'conectividade': 'OK',
            'segurança': 'OK',
            'ambiental': 'OK'
        }
        
        for sistema, status in sistemas.items():
            logger.info(f"  - {sistema.upper()}: {status}")
        
        logger.info("Diagnóstico concluído!")
    
    def relatorio_noturno(self):
        """Gera relatório das atividades noturnas"""
        if not self.ativo:
            return
        
        logger.info("Gerando relatório noturno...")
        
        relatorio = {
            'data': datetime.now().strftime('%d/%m/%Y'),
            'hora': datetime.now().strftime('%H:%M:%S'),
            'atividades_executadas': len(self.atividades_agendadas),
            'dispositivos_ativos': len([d for d in self.dispositivos.values() if d]),
            'status_geral': 'OPERACIONAL'
        }
        
        logger.info(f"Relatório: {relatorio}")
        self.salvar_relatorio(relatorio)
    
    def salvar_relatorio(self, relatorio: Dict):
        """Salva o relatório em arquivo"""
        try:
            with open('relatorio_noturno.txt', 'a', encoding='utf-8') as f:
                f.write(f"\n{'='*50}\n")
                f.write(f"Relatório: {relatorio['data']} - {relatorio['hora']}\n")
                f.write(f"Atividades Executadas: {relatorio['atividades_executadas']}\n")
                f.write(f"Dispositivos Ativos: {relatorio['dispositivos_ativos']}\n")
                f.write(f"Status: {relatorio['status_geral']}\n")
            logger.info("Relatório salvo com sucesso")
        except Exception as e:
            logger.error(f"Erro ao salvar relatório: {e}")
    
    def verificar_horario_noturno(self) -> bool:
        """Verifica se está no horário noturno (20h às 06h)"""
        hora_atual = datetime.now().hour
        return hora_atual >= 20 or hora_atual < 6
    
    def executar(self):
        """Executa o loop principal de automação"""
        logger.info("Iniciando sistema de automação...")
        
        try:
            while True:
                if self.verificar_horario_noturno() and not self.ativo:
                    self.iniciar_expansao_noturna()
                elif not self.verificar_horario_noturno() and self.ativo:
                    self.encerrar_expansao_noturna()
                
                schedule.run_pending()
                time.sleep(60)
        
        except KeyboardInterrupt:
            logger.info("Sistema interrompido pelo usuário")
            self.encerrar_expansao_noturna()
        except Exception as e:
            logger.error(f"Erro no sistema de automação: {e}")


def main():
    """Função principal"""
    automacao = SalaFuturoAutomacao()
    automacao.executar()


if __name__ == "__main__":
    main()
