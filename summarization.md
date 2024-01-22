{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": [],
      "authorship_tag": "ABX9TyOpvd6BI0YB4PYugr57joSB",
      "include_colab_link": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/VarshaLenin12/spaCy_Summarization/blob/main/summarization.md\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "code",
      "execution_count": null,
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "ZnYgvCkSGEwb",
        "outputId": "310da3e1-b1b0-49dd-9c85-9ca910324bc7"
      },
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Requirement already satisfied: spacy in /usr/local/lib/python3.10/dist-packages (3.6.1)\n",
            "Requirement already satisfied: spacy-legacy<3.1.0,>=3.0.11 in /usr/local/lib/python3.10/dist-packages (from spacy) (3.0.12)\n",
            "Requirement already satisfied: spacy-loggers<2.0.0,>=1.0.0 in /usr/local/lib/python3.10/dist-packages (from spacy) (1.0.5)\n",
            "Requirement already satisfied: murmurhash<1.1.0,>=0.28.0 in /usr/local/lib/python3.10/dist-packages (from spacy) (1.0.10)\n",
            "Requirement already satisfied: cymem<2.1.0,>=2.0.2 in /usr/local/lib/python3.10/dist-packages (from spacy) (2.0.8)\n",
            "Requirement already satisfied: preshed<3.1.0,>=3.0.2 in /usr/local/lib/python3.10/dist-packages (from spacy) (3.0.9)\n",
            "Requirement already satisfied: thinc<8.2.0,>=8.1.8 in /usr/local/lib/python3.10/dist-packages (from spacy) (8.1.12)\n",
            "Requirement already satisfied: wasabi<1.2.0,>=0.9.1 in /usr/local/lib/python3.10/dist-packages (from spacy) (1.1.2)\n",
            "Requirement already satisfied: srsly<3.0.0,>=2.4.3 in /usr/local/lib/python3.10/dist-packages (from spacy) (2.4.8)\n",
            "Requirement already satisfied: catalogue<2.1.0,>=2.0.6 in /usr/local/lib/python3.10/dist-packages (from spacy) (2.0.10)\n",
            "Requirement already satisfied: typer<0.10.0,>=0.3.0 in /usr/local/lib/python3.10/dist-packages (from spacy) (0.9.0)\n",
            "Requirement already satisfied: pathy>=0.10.0 in /usr/local/lib/python3.10/dist-packages (from spacy) (0.11.0)\n",
            "Requirement already satisfied: smart-open<7.0.0,>=5.2.1 in /usr/local/lib/python3.10/dist-packages (from spacy) (6.4.0)\n",
            "Requirement already satisfied: tqdm<5.0.0,>=4.38.0 in /usr/local/lib/python3.10/dist-packages (from spacy) (4.66.1)\n",
            "Requirement already satisfied: numpy>=1.15.0 in /usr/local/lib/python3.10/dist-packages (from spacy) (1.23.5)\n",
            "Requirement already satisfied: requests<3.0.0,>=2.13.0 in /usr/local/lib/python3.10/dist-packages (from spacy) (2.31.0)\n",
            "Requirement already satisfied: pydantic!=1.8,!=1.8.1,<3.0.0,>=1.7.4 in /usr/local/lib/python3.10/dist-packages (from spacy) (1.10.13)\n",
            "Requirement already satisfied: jinja2 in /usr/local/lib/python3.10/dist-packages (from spacy) (3.1.3)\n",
            "Requirement already satisfied: setuptools in /usr/local/lib/python3.10/dist-packages (from spacy) (67.7.2)\n",
            "Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.10/dist-packages (from spacy) (23.2)\n",
            "Requirement already satisfied: langcodes<4.0.0,>=3.2.0 in /usr/local/lib/python3.10/dist-packages (from spacy) (3.3.0)\n",
            "Requirement already satisfied: pathlib-abc==0.1.1 in /usr/local/lib/python3.10/dist-packages (from pathy>=0.10.0->spacy) (0.1.1)\n",
            "Requirement already satisfied: typing-extensions>=4.2.0 in /usr/local/lib/python3.10/dist-packages (from pydantic!=1.8,!=1.8.1,<3.0.0,>=1.7.4->spacy) (4.5.0)\n",
            "Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests<3.0.0,>=2.13.0->spacy) (3.3.2)\n",
            "Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.10/dist-packages (from requests<3.0.0,>=2.13.0->spacy) (3.6)\n",
            "Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests<3.0.0,>=2.13.0->spacy) (2.0.7)\n",
            "Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.10/dist-packages (from requests<3.0.0,>=2.13.0->spacy) (2023.11.17)\n",
            "Requirement already satisfied: blis<0.8.0,>=0.7.8 in /usr/local/lib/python3.10/dist-packages (from thinc<8.2.0,>=8.1.8->spacy) (0.7.11)\n",
            "Requirement already satisfied: confection<1.0.0,>=0.0.1 in /usr/local/lib/python3.10/dist-packages (from thinc<8.2.0,>=8.1.8->spacy) (0.1.4)\n",
            "Requirement already satisfied: click<9.0.0,>=7.1.1 in /usr/local/lib/python3.10/dist-packages (from typer<0.10.0,>=0.3.0->spacy) (8.1.7)\n",
            "Requirement already satisfied: MarkupSafe>=2.0 in /usr/local/lib/python3.10/dist-packages (from jinja2->spacy) (2.1.3)\n"
          ]
        }
      ],
      "source": [
        "pip install spacy"
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "import spacy\n",
        "from collections import Counter\n",
        "from heapq import nlargest\n",
        "\n",
        "# Load spaCy English model\n",
        "nlp = spacy.load(\"en_core_web_sm\")\n",
        "\n",
        "def preprocess_text(text):\n",
        "    # Tokenize and lemmatize the text using spaCy\n",
        "    doc = nlp(text)\n",
        "    lemmatized_tokens = [token.lemma_ for token in doc if token.is_alpha and not token.is_stop]\n",
        "    return lemmatized_tokens\n",
        "\n",
        "def extractive_summarize(text, num_sentences=3):\n",
        "    # Tokenize and preprocess the input text\n",
        "    tokens = preprocess_text(text)\n",
        "\n",
        "    # Calculate word frequency using Counter\n",
        "    word_freq = Counter(tokens)\n",
        "\n",
        "    # Calculate sentence scores based on word frequency\n",
        "    sentence_scores = {i: sum(word_freq[word] for word in preprocess_text(sentence)) for i, sentence in enumerate(text.split('\\n'))}\n",
        "\n",
        "    # Get the indices of the top-ranked sentences\n",
        "    top_sentences = nlargest(num_sentences, sentence_scores, key=sentence_scores.get)\n",
        "\n",
        "    # Sort the selected sentences based on their indices\n",
        "    selected_sentences = sorted(top_sentences)\n",
        "\n",
        "    # Sort the selected sentences in the original order\n",
        "    summary = '\\n'.join(sentence for i, sentence in enumerate(text.split('\\n')) if i in selected_sentences)\n",
        "\n",
        "    return summary\n",
        "\n",
        "# Example usage\n",
        "input_text = \"\"\"\n",
        "In shadows deep, where dreams reside,\n",
        "A quiet echo, whispers inside.\n",
        "Inside the night, a silent song,\n",
        "Song of stars, dancing along.\n",
        "\n",
        "Along the velvet sky, they gleam,\n",
        "Gleam with hope, a timeless theme.\n",
        "Theme of love, a tender rhyme,\n",
        "Rhyme of forever, beyond time.\n",
        "\"\"\"\n",
        "\n",
        "num_sentences = 5\n",
        "\n",
        "summary = extractive_summarize(input_text, num_sentences=num_sentences)\n",
        "print(\"Original Text:\\n\", input_text)\n",
        "print(\"\\nExtractive Summarized Text:\\n\", summary)\n"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "UnAns9UgGLKH",
        "outputId": "37b486ae-ed86-437b-be13-30d3f3a947b0"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Original Text:\n",
            " \n",
            "In shadows deep, where dreams reside,\n",
            "A quiet echo, whispers inside.\n",
            "Inside the night, a silent song,\n",
            "Song of stars, dancing along.\n",
            "\n",
            "Along the velvet sky, they gleam,\n",
            "Gleam with hope, a timeless theme.\n",
            "Theme of love, a tender rhyme,\n",
            "Rhyme of forever, beyond time.\n",
            "\n",
            "\n",
            "Extractive Summarized Text:\n",
            " In shadows deep, where dreams reside,\n",
            "A quiet echo, whispers inside.\n",
            "Inside the night, a silent song,\n",
            "Gleam with hope, a timeless theme.\n",
            "Theme of love, a tender rhyme,\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [],
      "metadata": {
        "id": "3J6kqpdFG6Kc"
      },
      "execution_count": null,
      "outputs": []
    }
  ]
}