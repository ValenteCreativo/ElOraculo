// CONSTS FOR THE CODE 

const contract = "guest-book.near";

const total_messages = Near.view(contract, "total_messages");

const messages = Near.view(contract, "get_messages", {
  from_index: total_messages - 10,
}).reverse();

State.init({
  new_message: "",
  author: null,
  quote: null,
  img: null,
});

const fetchQuote = () => {
  asyncFetch("https://stoic.tekloon.net/stoic-quote", {
    method: "GET",
    headers: {
      accept: "application/json",
      "content-type": "application/json",
    },
  })
    .then(({ body }) => {
      console.log(body);
      State.update({ author: body.author, quote: body.quote });
    })
    .catch((err) => console.log(err));
};

asyncFetch("https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY")
  .then(({ body }) => {
    console.log(body);
    State.update({ img: body.url });
  })
  .catch((err) => console.log(err));

const handleGuestBookEntry = async () => {
  const newMessage = State.get("new_message").text; // Extraer el texto del mensaje

  console.log("Valor del nuevo mensaje:", newMessage);

  if (!newMessage) {
    console.log("El mensaje no debe estar vacío");
    return;
  }

  try {
    await Near.call(contract, "add_message", {
      text: newMessage,
    });

    console.log("Mensaje añadido con éxito al libro de visitas");
  } catch (error) {
    console.error("Error al agregar el mensaje al libro de visitas:", error);
  }

  // Limpiar el campo de entrada después de agregar el mensaje
  State.update({ new_message: { text: "" } });
};

const shareOnTwitter = () => {
  const textToShare = `¡El oráculo dice: "${state.quote}" - ${
    state.author || "Autor Desconocido"
  }!`;
  const twitterShareUrl = `https://twitter.com/intent/tweet?text=${encodeURIComponent(
    textToShare
  )}`;

  window.open(twitterShareUrl, "_blank");
};

// STYLE 

const Main = styled.div`
  width: 100%;
  height: 750px;
  background: linear-gradient(135deg, #003d96, #01762F);
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 6px solid #01762F;
  border-radius: 20px;
`;

const Header = styled.div`
  width: 100%;
  height: 40px;
  background: linear-gradient(135deg, #032049, #7323b4);
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  box-shadow: 15px 20px 30px -10px #7323b4;
  color: #FFFFFF;
`;

const InputContainer = styled.div`
  margin-top: 20px;
`;

const ButtonContainer = styled.div`
  margin-top: 15px;
`;

const QuoteContainer = styled.div`
  margin-top: 10px;
  color: #FFFFFF;
  text-align: center;
`;

const GuestBookSection = styled.div`
  margin-top: 40px;
  width: 80%;
  background: linear-gradient(135deg, #003d96, #01762F);
  border-radius: 10px;
  padding: 10px;
  color: #FFFFFF;
`;

const GuestBookInput = styled.input`
  margin-bottom: 10px;
  width: 100%;
  padding: 8px;
  background-color: #FFFFFF;
  border: 2px solid #01762F;
  border-radius: 5px;
`;

const GuestBookButton = styled.button`
  width: 100%;
  padding: 8px;
  background-color: #01762F;
  color: #FFFFFF;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  transition: background-color 0.3s ease;

  &:hover {
    background-color: #014019;
  }
`;

// USER INTERFACE 

return (
  <Main>
    <Header style={{ textAlign: "center", marginTop: "2px" }}>
      <h4>¿Qué mensaje tiene el oráculo para ti hoy?</h4>
    </Header>

    <InputContainer>
      <input
        type="text"
        placeholder="¿Qué pregunta quieres hacer?"
        className="form-control"
        style={{ height: "100px", width: "250px" }}
      />
    </InputContainer>

    <ButtonContainer>
      <button
        className="btn btn-success mb-2"
        onClick={fetchQuote}
        style={{ fontSize: "16px" }}
      >
        Respuesta del universo
      </button>
    </ButtonContainer>

    {state.img != null && (
      <div style={{ marginTop: "7px", marginBottom: "1px" }}>
        <img
          src={state.img}
          alt="NASA"
          style={{ width: "100%", height: "140px" }}
        />
      </div>
    )}

    {state.quote != null && (
      <QuoteContainer style={{ marginTop: "10px" }}>
        <p>{state.quote}</p>
      </QuoteContainer>
    )}

    <GuestBookSection>
      <h4>¡Comparte tu frase!</h4>
      <GuestBookInput
        type="text"
        placeholder="¡Regala un mensaje positivo! "
        className="form-control"
        value={State.get("new_message").text}
        onChange={(e) =>
          State.update({ new_message: { text: e.target.value } })
        }
      />

      <GuestBookButton onClick={handleGuestBookEntry}>
        Dejar Mensaje
      </GuestBookButton>
    </GuestBookSection>

    <ButtonContainer>
      <button
        className="btn btn-info"
        style={{ fontSize: "14px", marginTop: "202px" }}
        onClick={shareOnTwitter}
      >
        Compartir en Twitter
      </button>
    </ButtonContainer>
  </Main>
);
