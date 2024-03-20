# pagina-prof
import './validacao.css'
import Lista from '../ListaAlunos';
import { useContext, useEffect, useState } from 'react';
import { AuthContext } from '../../../../../context/AuthContext';
import { Await } from 'react-router';
import axiosInstance from '../../../../../api/axiosInstance';


const Validar = () => {
    const { user } = useContext(AuthContext);

    const [dataSource, setDataSource] = useState(Array.from({length:20}));
    const [hasMore, setHasMore] = useState(true);
const [formVisible,setFormVisible]= useState(false);
const [frequenciaIndex, setFrequenciaIndex] = useState(0);

    const fetchMoreData = () => {
        if(dataSource.length < 40){
            setTimeout(() => {
                setDataSource(dataSource.concat(Array.from({length:20})))
            }, 500);
        }
        else{
            const [hasMore, setHasMore] = useState(false);
        }
    }
    
    const handleMonthClick = (meSelecionado) => {
        setFrequenciaIndex(meSelecionado);
        setFrequencia(prev => ({
            ...prev, 
            mes: meSelecionado,
        }));
        setFormVisible((prevVisible) => !prevVisible);
    };

    const onSubmit = (Data) => {
        console.log('Dados Submetidos:', Data);
    }
    const[frequencia,setFrequencia] = useState([]);
     const getFrequencia =async()=>{
        const res = await axiosInstance.get(`/frequencias/professor/${user?.id}`);
        console.log(res.data);
        console.log(res.data.length);
        return res.data;
      }
      useEffect(() =>{
        setFrequencia(getFrequencia());

      },
      [])

     return (
        <div>
          <div className="informacoes">
            <p>{user?.nome}</p>
            <p>Siape: {user?.siape}</p>
          </div>
      
          <div className='lista'>
            <div className='titulo_lista'>
              <p>INBOX</p>
            </div>
          </div>
      
          <div className="alunos">
            {Array.from({ length: 12 }, (_, index) => {
              const monthIndex = index + 1; // Adicionando 1 para começar em janeiro (1)
              
              return (
                <button
                  key={index}
                  className='botoes'
                  onClick={() => handleMonthClick(monthIndex)} // Passando monthIndex em vez de index + 1
                >
                  {index}
                </button>
              );
            })}
          </div>

            {
                formVisible &&(
                    <div className='formulario'>
                    <form id='itens' className='forms' >
                    
                        <label htmlFor="horasTrabalhadasMensal">
                            Horas feitas no mês(obrigatório)
                        </label>
                        <input 
                            type="text" 
                            id='horasTrabalhadasMensal'
                            name='horasTrabalhadasMensal'
                            value={frequencia[frequenciaIndex][horas_trabalhadas_mensal]}
                          readOnly 
                        />
                        <label htmlFor="observacao">
                            Relatório(obrigatório)
                        </label>
                        <input 
                            type='text'
                            id='observacao'
                            name='observacao'
                            readOnly
                        />
                        <div className='ajuste_botao'>
                            <button type='submit'>Enviar</button>
                        </div>
                    </form>
                    </div>

                )
            }

        </div>
      );
        }
        export default Validar;
