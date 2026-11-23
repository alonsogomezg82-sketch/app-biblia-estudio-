import streamlit as st
import random

# ---------------------- CONFIGURACIÓN ----------------------
st.set_page_config(
    page_title="📖 Aprende y Memoriza la Biblia",
    page_icon="✝️",
    layout="wide"
)

# ---------------------- BASE DE DATOS ----------------------
biblia_data = {
    "Génesis": {
        "descripcion": "El primer libro de la Biblia. Narra la creación del mundo, la caída del hombre y los patriarcas.",
        "imagen": "https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=600",
        "capitulos": {
            1: {
                "versiculo_clave": "Génesis 1:1 — En el principio creó Dios los cielos y la tierra.",
                "texto_completo": "En el principio creó Dios los cielos y la tierra. Y la tierra estaba desordenada y vacía, y las tinieblas estaban sobre la faz del abismo, y el Espíritu de Dios se movía sobre la faz de las aguas. Y dijo Dios: Sea la luz; y fue la luz.",
                "preguntas": [
                    {
                        "pregunta": "¿Qué creó Dios en el principio?",
                        "opciones": ["Solo la tierra", "Los cielos y la tierra", "Solo el cielo", "El sol y la luna"],
                        "respuesta_correcta": 1,
                        "explicacion": "En el principio creó Dios los cielos y la tierra. (Génesis 1:1)",
                        "concordancia": "Juan 1:3 — Todas las cosas por él fueron hechas, y sin él nada de lo que ha sido hecho, fue hecho."
                    },
                    {
                        "pregunta": "¿Qué dijo Dios que existiera primero?",
                        "opciones": ["El sol", "La luz", "Las estrellas", "El hombre"],
                        "respuesta_correcta": 1,
                        "explicacion": "Dijo Dios: Sea la luz; y fue la luz. (Génesis 1:3)",
                        "concordancia": "2 Corintios 4:6 — Porque Dios, que mandó que de las tinieblas resplandeciera la luz, es el que resplandeció en nuestros corazones..."
                    }
                ]
            },
            3: {
                "versiculo_clave": "Génesis 3:19 — Con el sudor de tu rostro comerás el pan hasta que vuelvas a la tierra.",
                "texto_completo": "Con el sudor de tu rostro comerás el pan hasta que vuelvas a la tierra; porque de ella fuiste tomado, pues polvo eres, y al polvo volverás.",
                "preguntas": [
                    {
                        "pregunta": "¿De qué fue formado el hombre?",
                        "opciones": ["Agua", "Polvo de la tierra", "Aire", "Fuego"],
                        "respuesta_correcta": 1,
                        "explicacion": "Polvo eres, y al polvo volverás. (Génesis 3:19)",
                        "concordancia": "Eclesiastés 12:7 — Y volverá el polvo a la tierra, como era, y el espíritu volverá a Dios que lo dio."
                    }
                ]
            }
        }
    },
    "Salmos": {
        "descripcion": "Colección de oraciones, alabanzas y cantos. Libro de fe y confianza en Dios.",
        "imagen": "https://images.unsplash.com/photo-1470114716349-090e4d921f92?w=600",
        "capitulos": {
            23: {
                "versiculo_clave": "Salmo 23:1 — Jehová es mi pastor; nada me faltará.",
                "texto_completo": "Jehová es mi pastor; nada me faltará. En lugares de delicados pastos me hará descansar; junto a aguas de reposo me pastoreará. Confortará mi alma; me guiará por sendas de justicia por amor de su nombre.",
                "preguntas": [
                    {
                        "pregunta": "¿Quién es nuestro pastor según este salmo?",
                        "opciones": ["El rey David", "Jehová", "Un ángel", "Un líder"],
                        "respuesta_correcta": 1,
                        "explicacion": "Jehová es mi pastor; nada me faltará. (Salmo 23:1)",
                        "concordancia": "Juan 10:11 — Yo soy el buen pastor; el buen pastor su vida da por las ovejas."
                    },
                    {
                        "pregunta": "¿Qué nos hace descansar?",
                        "opciones": ["En montañas altas", "En lugares de delicados pastos", "En ciudades grandes", "En el desierto"],
                        "respuesta_correcta": 1,
                        "explicacion": "En lugares de delicados pastos me hará descansar. (Salmo 23:2)",
                        "concordancia": "Mateo 11:28 — Venid a mí todos los que estáis trabajados y cargados, y yo os haré descansar."
                    }
                ]
            }
        }
    },
    "Mateo": {
        "descripcion": "El primer evangelio. Presenta a Jesús como el Mesías prometido y su enseñanza del Reino.",
        "imagen": "https://images.unsplash.com/photo-1507692316095-451d9f0b9e02?w=600",
        "capitulos": {
            5: {
                "versiculo_clave": "Mateo 5:8 — Bienaventurados los limpios de corazón, porque ellos verán a Dios.",
                "texto_completo": "Bienaventurados los pobres en espíritu, porque de ellos es el reino de los cielos. Bienaventurados los que lloran, porque ellos recibirán consolación. Bienaventurados los mansos, porque ellos recibirán la tierra por heredad... Bienaventurados los limpios de corazón, porque ellos verán a Dios.",
                "preguntas": [
                    {
                        "pregunta": "¿Quiénes verán a Dios?",
                        "opciones": ["Los fuertes", "Los limpios de corazón", "Los ricos", "Los sabios"],
                        "respuesta_correcta": 1,
                        "explicacion": "Bienaventurados los limpios de corazón, porque ellos verán a Dios. (Mateo 5:8)",
                        "concordancia": "Hebreos 12:14 — Seguid la paz con todos, y la santidad, sin la cual nadie verá al Señor."
                    }
                ]
            }
        }
    }
}

# ---------------------- ESTADO DE SESIÓN ----------------------
if "libro_seleccionado" not in st.session_state:
    st.session_state.libro_seleccionado = None
if "capitulo_actual" not in st.session_state:
    st.session_state.capitulo_actual = None
if "indice_pregunta" not in st.session_state:
    st.session_state.indice_pregunta = 0
if "puntuacion" not in st.session_state:
    st.session_state.puntuacion = 0
if "mostrar_respuesta" not in st.session_state:
    st.session_state.mostrar_respuesta = False
if "respuesta_usuario" not in st.session_state:
    st.session_state.respuesta_usuario = None

# ---------------------- FUNCIONES ----------------------
def reiniciar_pregunta():
    st.session_state.indice_pregunta = 0
    st.session_state.puntuacion = 0
    st.session_state.mostrar_respuesta = False
    st.session_state.respuesta_usuario = None

def siguiente_pregunta():
    st.session_state.indice_pregunta += 1
    st.session_state.mostrar_respuesta = False
    st.session_state.respuesta_usuario = None

# ---------------------- INTERFAZ PRINCIPAL ----------------------
st.title("✝️ 📖 Aprende y Memoriza la Biblia")
st.markdown("---")

pagina = st.sidebar.radio("📌 Navegación", ["🏠 Inicio", "📚 Estudiar por Libro", "🧠 Recordatorio Diario", "📖 Lista de Libros"])

if pagina == "🏠 Inicio":
    st.header("¡Bienvenido! 🙏")
    st.write("""
    Esta aplicación te ayuda a:
    - ✅ Navegar por cada libro de la Biblia
    - ✅ Leer versículos clave con su contexto completo
    - ✅ Responder preguntas para memorizar y recordar
    - ✅ Ver versículos relacionados (concordancia)
    - ✅ Ilustraciones que acompañan cada tema
    - ✅ Recordatorio diario con una pregunta
    """)
    st.info("💡 Ve a **'Estudiar por Libro'** para comenzar a estudiar. Cada capítulo tiene preguntas diseñadas para ayudarte a recordar lo que leíste.")
    st.image("https://images.unsplash.com/photo-1504052434569-70ad5836ab65?w=800", use_container_width=True)

elif pagina == "📖 Lista de Libros":
    st.header("📚 Libros Disponibles")
    for nombre_libro, datos in biblia_data.items():
        with st.expander(f"📖 {nombre_libro}"):
            st.write(datos["descripcion"])
            st.image(datos["imagen"], width=300)
            st.write(f"📝 Capítulos con preguntas: {list(datos['capitulos'].keys())}")

elif pagina == "📚 Estudiar por Libro":
    st.header("📚 Estudiar por Libro y Capítulo")
    
    lista_libros = list(biblia_data.keys())
    libro_idx = lista_libros.index(st.session_state.libro_seleccionado) if st.session_state.libro_seleccionado in lista_libros else 0
    st.session_state.libro_seleccionado = st.selectbox("📖 Selecciona un libro:", lista_libros, index=libro_idx)
    
    if st.session_state.libro_seleccionado:
        libro = biblia_data[st.session_state.libro_seleccionado]
        st.subheader(f"📖 {st.session_state.libro_seleccionado}")
        st.image(libro["imagen"], width=400)
        st.write(libro["descripcion"])
        
        lista_capitulos = list(libro["capitulos"].keys())
        capitulo_idx = lista_capitulos.index(st.session_state.capitulo_actual) if st.session_state.capitulo_actual in lista_capitulos else 0
        st.session_state.capitulo_actual = st.selectbox("📝 Selecciona un capítulo:", lista_capitulos, index=capitulo_idx, format_func=lambda x: f"Capítulo {x}")
        
        if st.session_state.capitulo_actual:
            capitulo = libro["capitulos"][st.session_state.capitulo_actual]
            st.markdown("---")
            
            st.subheader("✨ Versículo Clave")
            st.success(f"📌 **{capitulo['versiculo_clave']}**")
            
            with st.expander("📖 Leer capítulo completo (fragmento)"):
                st.write(capitulo["texto_completo"])
            
            st.markdown("---")
            st.subheader("🧠 Preguntas para Memorizar")
            
            preguntas = capitulo["preguntas"]
            total_preguntas = len(preguntas)
            
            if st.session_state.indice_pregunta < total_preguntas:
                pregunta_actual = preguntas[st.session_state.indice_pregunta]
                
                st.markdown(f"**Pregunta {st.session_state.indice_pregunta + 1} de {total_preguntas}:**")
                st.markdown(f"### ❓ {pregunta_actual['pregunta']}")
                
                if not st.session_state.mostrar_respuesta:
                    st.session_state.respuesta_usuario = st.radio("Selecciona tu respuesta:", pregunta_actual["opciones"], key=f"preg_{st.session_state.indice_pregunta}")
                    
                    if st.button("✅ Verificar respuesta"):
                        indice_respuesta = pregunta_actual["opciones"].index(st.session_state.respuesta_usuario)
                        st.session_state.mostrar_respuesta = True
                        
                        if indice_respuesta == pregunta_actual["respuesta_correcta"]:
                            st.session_state.puntuacion += 1
                            st.success("🎉 ¡Correcto! ¡Bien recordado! ✅")
                        else:
                            correcta = pregunta_actual["opciones"][pregunta_actual["respuesta_correcta"]]
                            st.error(f"❌ No es correcto. La respuesta correcta es: **{correcta}**")
                        st.rerun()
                
                if st.session_state.mostrar_respuesta:
                    st.info(f"📖 **Explicación:** {pregunta_actual['explicacion']}")
                    st.markdown(f"🔗 **Versículo relacionado (concordancia):** *{pregunta_actual['concordancia']}*")
                    
                    if st.button("➡️ Siguiente pregunta"):
                        siguiente_pregunta()
                        st.rerun()
            else:
                st.balloons()
                st.success(f"### 🎊 ¡Terminaste! Puntuación: {st.session_state.puntuacion} de {total_preguntas}")
                st.write("📖 Repasa el versículo clave una vez más:")
                st.success(f"✨ **{capitulo['versiculo_clave']}**")
                st.info("💡 *'El que medita en la Palabra día y noche, será bienaventurado.' — Salmo 1:2-3*")
                
                if st.button("🔄 Volver a intentar"):
                    reiniciar_pregunta()
                    st.rerun()

elif pagina == "🧠 Recordatorio Diario":
    st.header("⏰ Recordatorio — ¿Lo Recuerdas?")
    st.markdown("""
    Cada vez que entres aquí, tendrás una **pregunta al azar** para recordar lo que has estudiado. 
    ¡Es como un repaso para tu memoria! 🧠✨
    """)
    
    todas_las_preguntas = []
    for libro_nombre, libro in biblia_data.items():
        for capitulo_num, capitulo in libro["capitulos"].items():
            for pregunta in capitulo["preguntas"]:
                todas_las_preguntas.append({
                    "libro": libro_nombre,
                    "capitulo": capitulo_num,
                    "versiculo_clave": capitulo["versiculo_clave"],
                    "imagen": libro["imagen"],
                    **pregunta
                })
    
    if "pregunta_diaria" not in st.session_state or st.button("🔄 Otra pregunta"):
        st.session_state.pregunta_diaria = random.choice(todas_las_preguntas)
    
    p = st.session_state.pregunta_diaria
    
    st.markdown(f"📖 **{p['libro']} — Capítulo {p['capitulo']}**")
    st.image(p["imagen"], width=300)
    st.markdown(f"### ❓ {p['pregunta']}")
    
    respuesta_usuario = st.radio("¿Recuerdas la respuesta?", p["opciones"], key="diaria")
    
    if st.button("✅ Comprobar"):
        idx = p["opciones"].index(respuesta_usuario)
        if idx == p["respuesta_correcta"]:
            st.success("🎉 ¡Excelente! ¡Lo recordaste bien! 🙏✨")
        else:
            correcta = p["opciones"][p["respuesta_correcta"]]
            st.error(f"💭 No te preocupes, repasa: **{correcta}**")
        
        st.markdown("---")
        st.info(f"📖 **Versículo a recordar:** {p['versiculo_clave']}")
        st.markdown(f"🔗 **Versículo relacionado:** {p['concordancia']}")
        st.markdown("💛 *'Guarda mi palabra en tu corazón, para no pecar contra ti.' — Salmo 119:11*")
# app-biblia-estudio-